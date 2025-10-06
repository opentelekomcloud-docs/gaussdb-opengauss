:original_name: opengauss_opti_0046.html

.. _opengauss_opti_0046:

Tuning Statement Pushdown
=========================

Statement Pushdown
------------------

Currently, the GaussDB optimizer can use three methods to develop statement execution policies in the distributed framework: generating a statement pushdown plan, a distributed execution plan, or a distributed execution plan for sending statements.

-  A statement pushdown plan pushes query statements from a CN down to DNs for execution and returns the execution results to the CN.
-  In a distributed execution plan, a CN compiles and optimizes query statements, generates a plan tree, and then sends the plan tree to DNs for execution. After the statements have been executed, execution results will be returned to the CN.
-  A distributed execution plan for sending statements pushes queries that can be pushed down (mostly base table scanning statements) to DNs for execution. Then, the plan obtains the intermediate results and sends them to the CN, on which the remaining queries are to be executed.

The third method sends many intermediate results from DNs to the CN for further execution. In this case, the CN performance bottleneck (in bandwidth, storage, and compute) is caused by statements that cannot be pushed down to DNs. Therefore, you are not advised to use the query statements where only the third method applies.

Statements cannot be pushed down if they have :ref:`functions that do not support pushdown <en-us_topic_0000002124277309__section15430mcpsimp>` or :ref:`syntax that does not support pushdown <en-us_topic_0000002124277309__section15454mcpsimp>`. In this case, you can rewrite these execution statements.

Typical Scenarios of Statement Pushdown
---------------------------------------

Generally, no execution plan operator is displayed after the **EXPLAIN** statement. If the keyword similar to "Data Node Scan on" exists, statements have been pushed down to DNs for execution. The following describes statement pushdown and its supported scope from three scenarios.

**1. Pushdown of single-table query statements**

In a distributed database, to query a single table, whether the current statement can be pushed down depends on whether the CN needs to participate in calculation instead of simply collecting data. If the CN needs to further calculate the DN results, the statement cannot be pushed down. Generally, statements with keywords, such as **agg**, **windows function**, **limit/offset**, **sort**, or **distinct**, cannot be pushed down.

-  Pushdown: Simple queries can be pushed down without further calculation on the CN.

   .. code-block::

      postgres=# explain select * from t where c1 > 1;
                                       QUERY PLAN
      ----------------------------------------------------------------------------
       Data Node Scan on "__REMOTE_FQS_QUERY__"  (cost=0.00..0.00 rows=0 width=0)
         Node/s: All datanodes
      (2 rows)

-  Non-pushdown: A CN with the **limit** clause cannot simply send statements to DNs and collect data, which is inconsistent with the semantics of the **limit** clause.

   .. code-block::

      postgres=# explain select * from t limit 1;
                                           QUERY PLAN
      -------------------------------------------------------------------------------------
       Limit  (cost=0.00..0.00 rows=1 width=12)
         ->  Data Node Scan on "__REMOTE_LIMIT_QUERY__"  (cost=0.00..0.00 rows=1 width=12)
               Node/s: All datanodes
      (3 rows)

-  Non-pushdown: A CN with the **aggregate** function cannot simply push down statements. Instead, it needs to further aggregate the results collected from DNs.

   .. code-block::

      postgres=# explain select sum(c1), count(*) from t;
                                           QUERY PLAN
      -------------------------------------------------------------------------------------
       Aggregate  (cost=0.10..0.11 rows=1 width=20)
         ->  Data Node Scan on "__REMOTE_GROUP_QUERY__"  (cost=0.00..0.00 rows=20 width=4)
               Node/s: All datanodes
      (3 rows)

**2. Pushdown of multi-table query statements**

In a multi-table query, whether a statement can be pushed down depends on the **join** condition and distribution columns. If the **join** condition matches the distribution columns of the table, the statement can be pushed down. Otherwise, the statement cannot be pushed down. Generally, replication tables can be pushed down.

-  Create two hash distribution tables.

   .. code-block::

      postgres=# create table t(c1 int, c2 int, c3 int)distribute by hash(c1);
      CREATE TABLE
      postgres=# create table t1(c1 int, c2 int, c3 int)distribute by hash(c1);
      CREATE TABLE

-  Pushdown: The **join** condition meets the hash distribution column attributes of two tables.

   .. code-block::

      postgres=# explain select * from t1 join t on t.c1 = t1.c1;
                                       QUERY PLAN
      ----------------------------------------------------------------------------
       Data Node Scan on "__REMOTE_FQS_QUERY__"  (cost=0.00..0.00 rows=0 width=0)
         Node/s: All datanodes
      (2 rows)

-  Non-pushdown: The **join** condition does not meet the hash distribution column attribute. **t1.c2** is not the distribution column of **t1**.

   .. code-block::

      postgres=# explain select * from t1 join t on t.c1 = t1.c2;
                                               QUERY PLAN
      --------------------------------------------------------------------------------------------
       Hash Join  (cost=0.25..0.53 rows=20 width=24)
         Hash Cond: (t1.c2 = t.c1)
         ->  Data Node Scan on t1 "_REMOTE_TABLE_QUERY_"  (cost=0.00..0.00 rows=20 width=12)
               Node/s: All datanodes
         ->  Hash  (cost=0.00..0.00 rows=20 width=12)
               ->  Data Node Scan on t "_REMOTE_TABLE_QUERY_"  (cost=0.00..0.00 rows=20 width=12)
                     Node/s: All datanodes
      (7 rows)

**3. Special scenarios**

In some special scenarios, for example, a statement containing the **with recursive** clause or a column-store table cannot be pushed down.

Example: User-Defined Functions
-------------------------------

Define a user-defined function that generates fixed output for a certain input as the **immutable** type.

Take the sales information of TPCDS as an example. If you want to create a function to calculate the discount of a product, you can define the function as follows:

.. code-block::

   CREATE FUNCTION func_percent_2 (NUMERIC, NUMERIC) RETURNS NUMERIC
   AS 'SELECT $1 / $2 WHERE $2 > 0.01'
   LANGUAGE SQL
   VOLATILE;

Run the following statements:

.. code-block::

   SELECT func_percent_2(ss_sales_price, ss_list_price)
   FROM store_sales;

The execution plan is as follows.

|image1|

**func_percent_2** is not pushed down, and **ss_sales_price** and **ss_list_price** are executed on the CN. As a result, a large number of resources on the CN are consumed and the performance deteriorates.

In this example, the function generates the same output when the same input is provided. You can modify the function to the following one:

.. code-block::

   CREATE FUNCTION func_percent_1 (NUMERIC, NUMERIC) RETURNS NUMERIC
   AS 'SELECT $1 / $2 WHERE $2 > 0.01'
   LANGUAGE SQL
   IMMUTABLE;

Run the following statements:

.. code-block::

   SELECT func_percent_1(ss_sales_price, ss_list_price)
   FROM store_sales;

The execution plan is as follows.

|image2|

**func_percent_1** is pushed down to DNs for quicker execution. (In TPC-DS 1000X, where three CNs and 18 DNs are used, the query efficiency is improved by over 100 times).

.. _en-us_topic_0000002124277309__section15430mcpsimp:

Functions That Do Not Support Pushdown
--------------------------------------

The following describes the volatility of functions. The function volatility in GaussDB is as follows:

-  **IMMUTABLE**

   Specifies that the function always returns the same result if the parameter values are the same.

-  **STABLE**

   Specifies that the function cannot modify the database, and that within a single table scan, it will consistently return the same result for the same parameter value, but its result varies by SQL statements.

-  **VOLATILE**

   Specifies that the function value can change in a single table scan and no optimization is performed.

The volatility of a function can be obtained by querying its **provolatile** column in **pg_proc**. The value **i** indicates **IMMUTABLE**, **s** indicates **STABLE** and **v** indicates **VOLATILE**. The valid values of the **proshippable** column in **pg_proc** are **t**, **f**, and **NULL**. This column and the **provolatile** column together describe whether a function is pushed down.

-  If **provolatile** of a function is **i**, the function can be pushed down regardless of whether **proshippable** is **t**.
-  If **provolatile** of a function is **s** or **v**, the function can be pushed only if **proshippable** is **t**.
-  CTEs containing **random**, **exec_hadoop_sql**, or **exec_on_extension** are not pushed down, because pushdown may lead to incorrect results.

For a user-defined function, you can specify the values of **provolatile** and **proshippable** during its creation. For details, see CREATE FUNCTION.

For a function that does not support pushdown, you are advised to optimize it in the following ways:

-  If it is a system function, replace it with a functionally equivalent one.
-  If it is a user-defined function, check whether its **provolatile** and **proshippable** are correctly defined.

.. _en-us_topic_0000002124277309__section15454mcpsimp:

Syntax That Does Not Support Pushdown
-------------------------------------

SQL syntax that does not support pushdown is described using the following table definition examples:

.. code-block::

   postgres=# CREATE TABLE CUSTOMER1
   (
       C_CUSTKEY     BIGINT NOT NULL
     , C_NAME        VARCHAR(25) NOT NULL
     , C_ADDRESS     VARCHAR(40) NOT NULL
     , C_NATIONKEY   INT NOT NULL
     , C_PHONE       CHAR(15) NOT NULL
     , C_ACCTBAL     DECIMAL(15,2)   NOT NULL
     , C_MKTSEGMENT  CHAR(10) NOT NULL
     , C_COMMENT     VARCHAR(117) NOT NULL
   )
   DISTRIBUTE BY hash(C_CUSTKEY);
   postgres=# CREATE TABLE test_stream(a int,b float); --float does not support redistribution.
   postgres=# CREATE TABLE sal_emp ( c1 integer[] ) DISTRIBUTE BY replication;

-  The **RETURNING** statement cannot be pushed down.

   ::

      postgres=# explain update customer1 set C_NAME = 'a' returning c_name;
                                     QUERY PLAN
      ------------------------------------------------------------------
       Update on customer1  (cost=0.00..0.00 rows=30 width=187)
         Node/s: All datanodes
         Node expr: c_custkey
         ->  Data Node Scan on customer1 "_REMOTE_TABLE_QUERY_"  (cost=0.00..0.00 rows=30 width=187)
               Node/s: All datanodes
      (5 rows)

-  If a SQL statement contains the aggregate functions using **ORDER BY**, this statement cannot be pushed down.

   ::

      postgres=# explain verbose select count ( c_custkey order by c_custkey) from customer1;

                               QUERY PLAN
      ------------------------------------------------------------------ Aggregate  (cost=2.50..2.51 rows=1 width=8)
         Output: count(customer1.c_custkey ORDER BY customer1.c_custkey)
         ->  Data Node Scan on customer1 "_REMOTE_TABLE_QUERY_"  (cost=0.00..0.00 rows=30 width=8)
               Output: customer1.c_custkey
               Node/s: All datanodes
               Remote query: SELECT c_custkey FROM ONLY public.customer1 WHERE true
      (6 rows)

-  If a SQL statement contains **COUNT(DISTINCT** *expr*\ **)** and columns in **COUNT(DISTINCT** *expr*\ **)** do not support redistribution, this statement cannot be pushed down.

   ::

      postgres=# explain verbose select count(distinct b) from test_stream;
                                                QUERY PLAN
      ------------------------------------------------------------------ Aggregate  (cost=2.50..2.51 rows=1 width=8)
         Output: count(DISTINCT test_stream.b)
         ->  Data Node Scan on test_stream "_REMOTE_TABLE_QUERY_"  (cost=0.00..0.00 rows=30 width=8)
               Output: test_stream.b
               Node/s: All datanodes
               Remote query: SELECT b FROM ONLY public.test_stream WHERE true
      (6 rows)

-  A statement containing **DISTINCT ON** cannot be pushed down.

   ::

      postgres=# explain verbose select distinct on (c_custkey) c_custkey from customer1 order by c_custkey;
                                                  QUERY PLAN
      ------------------------------------------------------------------ Unique  (cost=49.83..54.83 rows=30 width=8)
         Output: customer1.c_custkey
         ->  Sort  (cost=49.83..52.33 rows=30 width=8)
               Output: customer1.c_custkey
               Sort Key: customer1.c_custkey
               ->  Data Node Scan on customer1 "_REMOTE_TABLE_QUERY_"  (cost=0.00..0.00 rows=30 width=8)
                     Output: customer1.c_custkey
                     Node/s: All datanodes
                     Remote query: SELECT c_custkey FROM ONLY public.customer1 WHERE true
      (9 rows)

-  A statement containing array expressions cannot be pushed down.

   ::

      postgres=# explain verbose select array[c_custkey,1] from customer1 order by c_custkey;

                                QUERY PLAN
      ------------------------------------------------------------------ Sort  (cost=49.83..52.33 rows=30 width=8)
         Output: (ARRAY[customer1.c_custkey, 1::bigint]), customer1.c_custkey
         Sort Key: customer1.c_custkey
         ->  Data Node Scan on "__REMOTE_SORT_QUERY__"  (cost=0.00..0.00 rows=30 width=8)
               Output: (ARRAY[customer1.c_custkey, 1::bigint]), customer1.c_custkey
               Node/s: All datanodes
               Remote query: SELECT ARRAY[c_custkey, 1::bigint], c_custkey FROM ONLY public.customer1 WHERE true ORDER BY 2
      (7 rows)

-  Some statements containing **WITH RECURSIVE** cannot be pushed down in the current version. The following table describes the specific scenarios and causes.

+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| No.                   | Scenario                                                                                                                                            | Cause                                                                                                                                                                                 |
+=======================+=====================================================================================================================================================+=======================================================================================================================================================================================+
| 1                     | The query contains foreign tables.                                                                                                                  | LOG: SQL can't be shipped, reason: RecursiveUnion contains HDFS Table or ForeignScan is not shippable (In this table, **LOG** indicates the non-pushdown reason recorded in CN logs.) |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | In the current version, queries containing foreign tables do not support pushdown.                                                                                                    |
+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 2                     | Multiple node groups                                                                                                                                | LOG: SQL can't be shipped, reason: With-Recursive under multi-nodegroup scenario is not shippable                                                                                     |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | In the current version, pushdown is supported only when all base tables are stored and computed in the same Node Group.                                                               |
+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 3                     | **ALL** is not used for **UNION**. In this case, the return result is deduplicated.                                                                 | LOG: SQL can't be shipped, reason: With-Recursive does not contain "ALL" to bind recursive & none-recursive branches                                                                  |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | Example:                                                                                                                                                                              |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | .. code-block::                                                                                                                                                                       |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     |    WITH recursive t_result AS (                                                                                                                                                       |
|                       |                                                                                                                                                     |    SELECT dm,sj_dm,name,1 as level                                                                                                                                                    |
|                       |                                                                                                                                                     |    FROM test_rec_part                                                                                                                                                                 |
|                       |                                                                                                                                                     |    WHERE sj_dm > 10                                                                                                                                                                   |
|                       |                                                                                                                                                     |    UNION                                                                                                                                                                              |
|                       |                                                                                                                                                     |    SELECT t2.dm,t2.sj_dm,t2.name||' > '||t1.name,t1.level+1                                                                                                                           |
|                       |                                                                                                                                                     |    FROM t_result t1                                                                                                                                                                   |
|                       |                                                                                                                                                     |    JOIN test_rec_part t2 ON t2.sj_dm = t1.dm                                                                                                                                          |
|                       |                                                                                                                                                     |    )                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    SELECT * FROM t_result t;                                                                                                                                                          |
+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 4                     | A base table contains the system catalog.                                                                                                           | LOG: SQL can't be shipped, reason: With-Recursive contains system table is not shippable                                                                                              |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | Example:                                                                                                                                                                              |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | .. code-block::                                                                                                                                                                       |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     |    WITH RECURSIVE x(id) AS                                                                                                                                                            |
|                       |                                                                                                                                                     |    (                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    select count(1) from pg_class where oid=1247                                                                                                                                       |
|                       |                                                                                                                                                     |    UNION ALL                                                                                                                                                                          |
|                       |                                                                                                                                                     |    SELECT id+1 FROM x WHERE id < 5                                                                                                                                                    |
|                       |                                                                                                                                                     |    ), y(id) AS                                                                                                                                                                        |
|                       |                                                                                                                                                     |    (                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    select count(1) from pg_class where oid=1247                                                                                                                                       |
|                       |                                                                                                                                                     |    UNION ALL                                                                                                                                                                          |
|                       |                                                                                                                                                     |    SELECT id+1 FROM x WHERE id < 10                                                                                                                                                   |
|                       |                                                                                                                                                     |    )                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    SELECT y.*, x.* FROM y LEFT JOIN x USING (id) ORDER BY 1;                                                                                                                          |
+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 5                     | Only **VALUES** is used for scanning base tables. In this case, the statement can be executed only on the CN.                                       | LOG: SQL can't be shipped, reason: With-Recursive contains only values rte is not shippable                                                                                           |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | Example:                                                                                                                                                                              |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | .. code-block::                                                                                                                                                                       |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     |    WITH RECURSIVE t(n) AS (                                                                                                                                                           |
|                       |                                                                                                                                                     |    VALUES (1)                                                                                                                                                                         |
|                       |                                                                                                                                                     |    UNION ALL                                                                                                                                                                          |
|                       |                                                                                                                                                     |    SELECT n+1 FROM t WHERE n < 100                                                                                                                                                    |
|                       |                                                                                                                                                     |    )                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    SELECT sum(n) FROM t;                                                                                                                                                              |
+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 6                     | The correlation conditions of correlated subqueries are only in the recursion part, and the non-recursion part has no correlation condition.        | LOG: SQL can't be shipped, reason: With-Recursive recursive term correlated only is not shippable                                                                                     |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | Example:                                                                                                                                                                              |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | .. code-block::                                                                                                                                                                       |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     |    select  a.ID,a.Name,                                                                                                                                                               |
|                       |                                                                                                                                                     |    (                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    with recursive cte as (                                                                                                                                                            |
|                       |                                                                                                                                                     |    select ID, PID, NAME from b where b.ID = 1                                                                                                                                         |
|                       |                                                                                                                                                     |    union all                                                                                                                                                                          |
|                       |                                                                                                                                                     |    select parent.ID,parent.PID,parent.NAME                                                                                                                                            |
|                       |                                                                                                                                                     |    from cte as child join b as parent on child.pid=parent.id                                                                                                                          |
|                       |                                                                                                                                                     |    where child.ID = a.ID                                                                                                                                                              |
|                       |                                                                                                                                                     |    )                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    select NAME from cte limit 1                                                                                                                                                       |
|                       |                                                                                                                                                     |    ) cName                                                                                                                                                                            |
|                       |                                                                                                                                                     |    from                                                                                                                                                                               |
|                       |                                                                                                                                                     |    (                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    select id, name, count(*) as cnt                                                                                                                                                   |
|                       |                                                                                                                                                     |    from a group by id,name                                                                                                                                                            |
|                       |                                                                                                                                                     |    ) a order by 1,2;                                                                                                                                                                  |
+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 7                     | The **replicate** plan is used for **limit** in the non-recursion part but the **hash** plan is used in the recursion part, resulting in conflicts. | LOG: SQL can't be shipped, reason: With-Recursive contains conflict distribution in none-recursive(Replicate) recursive(Hash)                                                         |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | Example:                                                                                                                                                                              |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | .. code-block::                                                                                                                                                                       |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     |    WITH recursive t_result AS (                                                                                                                                                       |
|                       |                                                                                                                                                     |    select * from(                                                                                                                                                                     |
|                       |                                                                                                                                                     |    SELECT dm,sj_dm,name,1 as level                                                                                                                                                    |
|                       |                                                                                                                                                     |    FROM test_rec_part                                                                                                                                                                 |
|                       |                                                                                                                                                     |    WHERE sj_dm < 10 order by dm limit 6 offset 2)                                                                                                                                     |
|                       |                                                                                                                                                     |    UNION all                                                                                                                                                                          |
|                       |                                                                                                                                                     |    SELECT t2.dm,t2.sj_dm,t2.name||' > '||t1.name,t1.level+1                                                                                                                           |
|                       |                                                                                                                                                     |    FROM t_result t1                                                                                                                                                                   |
|                       |                                                                                                                                                     |    JOIN test_rec_part t2 ON t2.sj_dm = t1.dm                                                                                                                                          |
|                       |                                                                                                                                                     |    )                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    SELECT * FROM t_result t;                                                                                                                                                          |
+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 8                     | **recursive** of multiple layers are nested. A **recursive** is nested in the recursion part of another **recursive**.                              | LOG: SQL can't be shipped, reason: Recursive CTE references recursive CTE "cte"                                                                                                       |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | Example:                                                                                                                                                                              |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     | .. code-block::                                                                                                                                                                       |
|                       |                                                                                                                                                     |                                                                                                                                                                                       |
|                       |                                                                                                                                                     |    with recursive cte as                                                                                                                                                              |
|                       |                                                                                                                                                     |    (                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    select * from rec_tb4 where id<4                                                                                                                                                   |
|                       |                                                                                                                                                     |    union all                                                                                                                                                                          |
|                       |                                                                                                                                                     |    select h.id,h.parentID,h.name from                                                                                                                                                 |
|                       |                                                                                                                                                     |    (                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    with recursive cte as                                                                                                                                                              |
|                       |                                                                                                                                                     |    (                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    select * from rec_tb4 where id<4                                                                                                                                                   |
|                       |                                                                                                                                                     |    union all                                                                                                                                                                          |
|                       |                                                                                                                                                     |    select h.id,h.parentID,h.name from rec_tb4 h inner join cte c on h.id=c.parentID                                                                                                   |
|                       |                                                                                                                                                     |    )                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    SELECT id ,parentID,name from cte order by parentID                                                                                                                                |
|                       |                                                                                                                                                     |    ) h                                                                                                                                                                                |
|                       |                                                                                                                                                     |    inner join cte  c on h.id=c.parentID                                                                                                                                               |
|                       |                                                                                                                                                     |    )                                                                                                                                                                                  |
|                       |                                                                                                                                                     |    SELECT id ,parentID,name from cte order by parentID,1,2,3;                                                                                                                         |
+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. |image1| image:: /_static/images/en-us_image_0000002124197333.png
.. |image2| image:: /_static/images/en-us_image_0000002124197337.png
