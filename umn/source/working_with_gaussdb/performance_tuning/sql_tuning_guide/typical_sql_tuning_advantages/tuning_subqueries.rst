:original_name: opengauss_opti_0047.html

.. _opengauss_opti_0047:

Tuning Subqueries
=================

Background
----------

When an application runs a SQL statement to operate the database, a large number of subqueries are used because they are clearer than table join. Especially in complicated query statements, subqueries have more complete and independent semantics, which makes SQL statements clearer and easy to understand. Therefore, subqueries are widely used.

In GaussDB, subqueries can also be called sublinks based on the location of subqueries in SQL statements.

-  Subquery: corresponds to a range table (**RangeTblEntry**) in the query parse tree. Specifically, a subquery is a **SELECT** statement following immediately after the **FROM** keyword.

-  Sublink: corresponds to an expression in the query parsing tree. That is, a sublink is a statement in the **WHERE** or **ON** clause or in the target list.

   In conclusion, a subquery is a scope table and a sublink is an expression in the query parsing tree. A sublink can be found in constraints and expressions. In GaussDB, sublinks can be classified into the following types:

   -  **exist_sublink**: corresponds to the **EXIST** and **NOT EXIST** statements.
   -  **any_sublink**: corresponds to the *op* **ALL(SELECT...)** statement. *op* can be the **IN**, **<**, **>**, or **=** operator.
   -  **all_sublink**: corresponds to the *op* **ALL(SELECT...)** statement. *op* can be the **IN**, **<**, **>**, or **=** operator.
   -  **rowcompare_sublink**: corresponds to the **RECORD** *op* **(SELECT...)** statement.
   -  **expr_sublink**: corresponds to the **(SELECT** *with a single target list item...*\ **)** statement.
   -  **array_sublink**: corresponds to the **ARRAY(SELECT...)** statement.
   -  **cte_sublink**: corresponds to the **WITH(...)** query statement.

Sublink Optimization in GaussDB
-------------------------------

To optimize a sublink, a subquery is pulled up to join with tables in outer queries, preventing the subquery from being converted into a plan involving subplans and broadcast. You can run the **EXPLAIN** statement to check whether a sublink is converted into the combination of a subplan and broadcast.

Example:

|image1|

-  **Sublink-release scenarios supported by GaussDB**

   -  Pulling up the **IN** sublink

      -  The subquery cannot contain columns in the outer query (columns in more outer queries are allowed).
      -  The subquery cannot contain volatile functions.

      |image2|

   -  Pulling up the **EXISTS** sublink

      The **WHERE** clause must contain a column in the outer query. Other parts of the subquery cannot contain the column. Other restrictions are as follows:

      -  The subquery must contain the **FROM** clause.
      -  The subquery cannot contain the **WITH** clause.
      -  The subquery cannot contain aggregate functions.
      -  The subquery cannot contain a **SET**, **SORT**, **LIMIT**, **WindowAgg**, or **HAVING** operation.
      -  The subquery cannot contain volatile functions.

      |image3|

   -  Pulling up an equivalent correlated query containing aggregate functions

      The **WHERE** condition of the subquery must contain a column from the outer query. Equivalence comparison must be performed between this column and related columns in tables of the subquery. These conditions must be connected using **AND**. Other parts of the subquery cannot contain the column. Other restrictions are as follows:

      -  The columns in the expression in the **WHERE** condition of the subquery must exist in tables.

      -  After the **SELECT** keyword of the subquery, there must be only one output column. The output column must be an aggregate function (for example, **MAX**), and the parameter (for example, **t2.c2**) of the aggregate function cannot be columns of a table (for example, **t1**) in outer queries. The aggregate function cannot be **COUNT**.

         For example, the following subquery can be pulled up:

         ::

            select * from t1 where c1 >(
                   select max(t2.c1) from t2 where t2.c1=t1.c1
            );

         The following subquery cannot be pulled up because the subquery has no aggregate function:

         ::

            select * from t1 where c1 >(
                   select  t2.c1 from t2 where t2.c1=t1.c1
            );

         The following subquery cannot be pulled up because the subquery has two output columns:

         ::

            select * from t1 where (c1,c2) >(
                   select  max(t2.c1),min(t2.c2) from t2 where t2.c1=t1.c1
            );

      -  The subquery must be a **FROM** clause.

      -  The subquery cannot contain a **GROUP BY**, **HAVING**, or **SET** operation.

      -  The subquery can only be an inner join.

         For example, the following subquery cannot be pulled up:

         ::

            select * from t1 where c1 >(
                   select max(t2.c1) from t2 full join t3 on (t2.c2=t3.c2) where t2.c1=t1.c1
            );

      -  The target list of the subquery cannot contain the function that returns a set.

      -  The **WHERE** condition of the subquery must contain a column from the outer query. Equivalence comparison must be performed between this column and related columns in tables of the subquery. These conditions must be connected using **AND**. Other parts of the subquery cannot contain the column. For example, the following subquery can be pulled up:

         ::

            select * from t3 where t3.c1=(
                    select t1.c1
                    from t1 where c1 >(
                            select max(t2.c1) from t2 where t2.c1=t1.c1
            ));

         If another condition is added to the subquery in the previous example, the subquery cannot be pulled up because the subquery references to the column in the outer query. The following is an example:

         ::

            select * from t3 where t3.c1=(
                    select t1.c1
                    from t1 where c1 >(
                           select max(t2.c1) from t2 where t2.c1=t1.c1 and t3.c1>t2.c2

            ));

   -  Pulling up a sublink in the **OR** clause

      If the **WHERE** condition contains an **EXIST** correlated sublink connected by **OR**, for example:

      ::

         select a, c from t1
         where t1.a = (select avg(a) from t3 where t1.b = t3.b) or
         exists (select * from t4 where t1.c = t4.c);

      The process of pulling up such a sublink is as follows:

      #. Extract **opExpr** from the **OR** clause in the **WHERE** condition. The value is **t1.a = (select avg(a) from t3 where t1.b = t3.b)**.

      #. The **opExpr** contains a subquery. If the subquery can be pulled up, the subquery is rewritten as **select avg(a), t3.b from t3 group by t3.b**, generating the **NOT NULL** condition **t3.b is not null**. The **opExpr** is replaced with this **NOT NULL** condition. In this case, the SQL statement changes to:

         ::

            select a, c
            from t1 left join (select avg(a) avg, t3.b from t3 group by t3.b)  as t3 on (t1.a = avg and t1.b = t3.b)
            where t3.b is not null or exists (select * from t4 where t1.c = t4.c);

      #. Extract the **EXISTS** sublink **exists (select \* from t4 where t1.c = t4.c)** from the **OR** clause to check whether the sublink can be pulled up. If it can be pulled up, it is converted into **select t4.c from t4 group by t4.c**, generating the **NOT NULL** condition **t4.c is not null**. In this case, the SQL statement changes to:

         ::

            select t1.a, t1.c from t1 left join (select avg(a) avg, t3.b from t3 group by t3.b) as t3 on (t1.a = avg and t1.b = t3.b) left join (select t5.c from t5 group by t5.c) as t5 on (t1.c = t5.c) where t3.b is not null or t5.c is not null;

         |image4|

-  **Sublink-release scenarios not supported by GaussDB**

   Except the sublinks described above, all the other sublinks cannot be pulled up. In this case, a join subquery is planned as the combination of subplans and broadcast. As a result, if inner tables in the subquery have a large amount of data, query performance may be poor.

   If a correlated subquery joins with two tables in outer queries, the subquery cannot be pulled up. You need to change the outer query into a **WITH** clause and then perform the join.

   Example:

   ::

      select distinct t1.a, t2.a
      from t1 left join t2 on t1.a=t2.a and not exists (select a,b from test1 where test1.a=t1.a and test1.b=t2.a);

   Changed to:

   ::

      with temp as
      (
              select * from (select t1.a as a, t2.a as b from t1 left join t2 on t1.a=t2.a)
      )
      select distinct a,b
      from temp
      where not exists (select a,b from test1 where temp.a=test1.a and temp.b=test1.b);

   -  The subquery (without **COUNT**) in the target list cannot be pulled up.

      Example:

      ::

         explain (costs off)
         select (select c2 from t2 where t1.c1 = t2.c1) ssq, t1.c2
         from t1
         where t1.c2 > 10;

      The execution plan is as follows:

      ::

         explain (costs off)
         select (select c2 from t2 where t1.c1 = t2.c1) ssq, t1.c2
         from t1
         where t1.c2 > 10;
                               QUERY PLAN
         ------------------------------------------------------
          Streaming (type: GATHER)
            Node/s: All datanodes
            ->  Seq Scan on t1
                  Filter: (c2 > 10)
                  SubPlan 1
                    ->  Result
                          Filter: (t1.c1 = t2.c1)
                          ->  Materialize
                                ->  Streaming(type: BROADCAST)
                                      Spawn on: All datanodes
                                      ->  Seq Scan on t2
         (11 rows)

      The correlated subquery is displayed in the target list (query return list). Values need to be returned even if the condition **t1.c1=t2.c1** is not met. Therefore, use left outer join to join **T1** and **T2** so that SSQ can return padding values when the condition **t1.c1=t2.c1** is not met.

      .. note::

         ScalarSubQuery (SSQ) and Correlated-ScalarSubQuery (CSSQ) are described as follows:

         -  SSQ: a sublink that returns a scalar value of a single row with a single column
         -  CSSQ: an SSQ containing correlation conditions

      The preceding SQL statement can be changed into:

      ::

         with ssq as
         (
             select t2.c2 from t2
         )
         select ssq.c2, t1.c2
         from t1 left join ssq on t1.c1 = ssq.c2
         where t1.c2 > 10;

      The execution plan after the change is as follows:

      ::

                         QUERY PLAN
         -------------------------------------------
          Streaming (type: GATHER)
            Node/s: All datanodes
            ->  Hash Right Join
                  Hash Cond: (t2.c2 = t1.c1)
                  ->  Streaming(type: REDISTRIBUTE)
                        Spawn on: All datanodes
                        ->  Seq Scan on t2
                  ->  Hash
                        ->  Seq Scan on t1
                              Filter: (c2 > 10)
         (10 rows)

      In the preceding example, the SSQ in the target list is pulled up to right join, preventing poor performance caused by the plan involving subplans and broadcast when the table (**T2**) in the subquery is too large.

   -  The subquery (with **COUNT**) in the target list cannot be pulled up.

      Example:

      ::

         select (select count(*) from t2 where t2.c1=t1.c1) cnt, t1.c1, t3.c1
         from t1,t3
         where t1.c1=t3.c1 order by cnt, t1.c1;

      The execution plan is as follows:

      ::

                                     QUERY PLAN
         ------------------------------------------------------------------
          Streaming (type: GATHER)
            Node/s: All datanodes
            ->  Sort
                  Sort Key: ((SubPlan 1)), t1.c1
                  ->  Hash Join
                        Hash Cond: (t1.c1 = t3.c1)
                        ->  Seq Scan on t1
                        ->  Hash
                              ->  Seq Scan on t3
                        SubPlan 1
                          ->  Aggregate
                                ->  Result
                                      Filter: (t2.c1 = t1.c1)
                                      ->  Materialize
                                            ->  Streaming(type: BROADCAST)
                                                  Spawn on: All datanodes
                                                  ->  Seq Scan on t2
         (17 rows)

      The correlated subquery is displayed in the target list (query return list). Values need to be returned even if the condition **t1.c1=t2.c1** is not met. Therefore, use left outer join to join **T1** and **T2** so that SSQ can return padding values when the condition **t1.c1=t2.c1** is not met. However, **COUNT** is used, which requires that **0** is returned when the condition is not met. **case-when NULL then 0 else count(*)** can be used.

      The preceding SQL statement can be changed into:

      ::

         with ssq as
         (
             select count(*) cnt, c1 from t2 group by c1
         )
         select case when
                     ssq.cnt is null then 0
                     else ssq.cnt
                end cnt, t1.c1, t3.c1
         from t1 left join ssq on ssq.c1 = t1.c1,t3
         where t1.c1 = t3.c1
         order by ssq.cnt, t1.c1;

      The execution plan after the change is as follows:

      ::

           QUERY PLAN
         -----------------------------------------------------
          Streaming (type: GATHER)
            Node/s: All datanodes
            ->  Sort
                  Sort Key: (count(*)), t1.c1
                  ->  Hash Join
                        Hash Cond: (t1.c1 = t3.c1)
                        ->  Hash Left Join
                              Hash Cond: (t1.c1 = t2.c1)
                              ->  Seq Scan on t1
                              ->  Hash
                                    ->  HashAggregate
                                          Group By Key: t2.c1
                                          ->  Seq Scan on t2
                        ->  Hash
                              ->  Seq Scan on t3
         (15 rows)

   -  Non-equivalent correlated subqueries cannot be pulled up.

      Example:

      ::

         select t1.c1, t1.c2
         from t1
         where t1.c1 = (select agg() from t2.c2 > t1.c2);

      Non-equivalent correlated subqueries cannot be pulled up. You can perform two JOIN operations twice (one CorrelationKey and one rownum self-join) to rewrite the statement.

      You can rewrite the statement in either of the following ways:

      -  Subquery rewriting

         ::

            select t1.c1, t1.c2
            from t1, (
                select t1.rowid, agg() aggref
                from t1,t2
                where t1.c2 > t2.c2 group by t1.rowid
            ) dt /* derived table */
            where t1.rowid = dt.rowid AND t1.c1 = dt.aggref;

      -  CTE rewriting

         ::

            WITH dt as
            (
                select t1.rowid, agg() aggref
                from t1,t2
                where t1.c2 > t2.c2 group by t1.rowid
            )
            select t1.c1, t1.c2
            from t1, derived_table
            where t1.rowid = derived_table.rowid AND
            t1.c1 = derived_table.aggref;

   .. important::

      -  Currently, GaussDB does not have an effective way to provide globally unique row IDs for tables and intermediate result sets. Therefore, the rewriting is difficult. It is recommended that this issue is avoided at the service layer or by using **t1.xc_nodeid + t1.ctid** to join row IDs. However, the high repetition rate of **xc_nodeid** leads to low join efficiency, and **xc_node_id+ctid** cannot be used as the join condition of a hash join.
      -  If the AGG type is **COUNT(*)**, **0** is used for data padding when **CASE-WHEN** is not matched. If the type is not **COUNT(*)**, **NULL** is used.
      -  CTE rewriting works better by using ShareScan.

More Optimization Examples
--------------------------

**Example 1:** Change the base table to a replication table and create an index on the filter column.

.. code-block::

   create table master_table (a int);
   create table sub_table(a int, b int);
   select a from master_table group by a having a in (select a from sub_table);

In this example, a correlated subquery is contained. To improve the query performance, you can change **sub_table** to a replication table and create an index on the **a** column.

**Example 2:** Modify the SELECT statement by changing the subquery to a JOIN relationship between the primary table and the parent query or modifying the subquery to improve the query performance. Ensure that the subquery to be used is semantically correct.

.. code-block::

   explain (costs off)select * from master_table as t1 where t1.a in (select t2.a from sub_table as t2 where t1.a = t2.b);
                           QUERY PLAN
   ----------------------------------------------------------
    Streaming (type: GATHER)
      Node/s: All datanodes
      ->  Seq Scan on master_table t1
            Filter: (SubPlan 1)
            SubPlan 1
              ->  Result
                    Filter: (t1.a = t2.b)
                    ->  Materialize
                          ->  Streaming(type: BROADCAST)
                                Spawn on: All datanodes
                                ->  Seq Scan on sub_table t2
   (11 rows)

In the preceding example, a subplan is used. To remove the subplan, modify the statement as follows:

.. code-block::

   explain(costs off) select * from master_table as t1 where exists (select t2.a from sub_table as t2 where t1.a = t2.b and t1.a = t2.a);
                       QUERY PLAN
   --------------------------------------------------
    Streaming (type: GATHER)
      Node/s: All datanodes
      ->  Hash Semi Join
            Hash Cond: (t1.a = t2.b)
            ->  Seq Scan on master_table t1
            ->  Hash
                  ->  Streaming(type: REDISTRIBUTE)
                        Spawn on: All datanodes
                        ->  Seq Scan on sub_table t2
   (9 rows)

In this way, the subplan is replaced by the semi-join between the two tables, greatly improving the execution efficiency.

.. |image1| image:: /_static/images/en-us_image_0000002088677994.png
.. |image2| image:: /_static/images/en-us_image_0000002088518146.png
.. |image3| image:: /_static/images/en-us_image_0000002124197437.png
.. |image4| image:: /_static/images/en-us_image_0000002088518138.png
