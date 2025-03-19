:original_name: opengauss_opti_0050.html

.. _opengauss_opti_0050:

Tuning Data Skew
================

Data skew breaks the balance among nodes in the distributed MPP architecture. If the amount of data stored or processed by a node is much greater than that by other nodes, the following problems may occur:

-  Storage skew severely limits the system capacity. The skew on a single node hinders system storage utilization.
-  Computing skew severely affects performance. The data to be processed on the skew node is much more than that on other nodes, deteriorating overall system performance.
-  Data skew severely affects the scalability of the MPP architecture. During storage or computing, data with the same values is often placed on the same node. Therefore, even if you add nodes after a data skew occurs, the skew data (data with the same values) is still placed on the node and affects the system capacity or performance bottleneck.

GaussDB provides a complete solution for data skew, including storage and computing skew.

Data Skew in the Storage Layer
------------------------------

In the GaussDB database, data is distributed and stored on each DN. You can improve the query efficiency by using distributed execution. However, if data skew occurs, bottlenecks exist on some DNs during distribution execution, affecting the query performance. This is because the distribution column is not properly selected. This can be solved by adjusting the distribution column.

For example:

.. code-block::

   postgres=# explain performance select count(*) from inventory;
   5 --CStore Scan on lmz.inventory
            dn_6001_6002 (actual time=0.444..83.127 rows=42000000 loops=1)
            dn_6003_6004 (actual time=0.512..63.554 rows=27000000 loops=1)
            dn_6005_6006 (actual time=0.722..99.033 rows=45000000 loops=1)
            dn_6007_6008 (actual time=0.529..100.379 rows=51000000 loops=1)
            dn_6009_6010 (actual time=0.382..71.341 rows=36000000 loops=1)
            dn_6011_6012 (actual time=0.547..100.274 rows=51000000 loops=1)
            dn_6013_6014 (actual time=0.596..118.289 rows=60000000 loops=1)
            dn_6015_6016 (actual time=1.057..132.346 rows=63000000 loops=1)
            dn_6017_6018 (actual time=0.940..110.310 rows=54000000 loops=1)
            dn_6019_6020 (actual time=0.231..41.198 rows=21000000 loops=1)
            dn_6021_6022 (actual time=0.927..114.538 rows=54000000 loops=1)
            dn_6023_6024 (actual time=0.637..118.385 rows=60000000 loops=1)
            dn_6025_6026 (actual time=0.288..32.240 rows=15000000 loops=1)
            dn_6027_6028 (actual time=0.566..118.096 rows=60000000 loops=1)
            dn_6029_6030 (actual time=0.423..82.913 rows=42000000 loops=1)
            dn_6031_6032 (actual time=0.395..78.103 rows=39000000 loops=1)
            dn_6033_6034 (actual time=0.376..51.052 rows=24000000 loops=1)
            dn_6035_6036 (actual time=0.569..79.463 rows=39000000 loops=1)

In the performance information, you can view the number of scan rows of each DN in the inventory table. The number of rows of each DN differs a lot. The maximum number of rows is 63,000,000, and the minimum number of rows is 15,000,000. The difference is four times. This value difference on the performance of data scan is acceptable, but if the join operator exists in the upper-layer, the impact on the performance cannot be ignored.

Generally, the data table is hash distributed on each DN; therefore, it is important to choose a proper distribution key. Use **table_skewness()** to view the data skewness of each DN in the inventory table. The query result is as follows:

.. code-block::

   postgres=# select table_skewness('inventory');
                 table_skewness
   ------------------------------------------
    ("dn_6015_6016        ",63000000,8.046%)
    ("dn_6013_6014        ",60000000,7.663%)
    ("dn_6023_6024        ",60000000,7.663%)
    ("dn_6027_6028        ",60000000,7.663%)
    ("dn_6017_6018        ",54000000,6.897%)
    ("dn_6021_6022        ",54000000,6.897%)
    ("dn_6007_6008        ",51000000,6.513%)
    ("dn_6011_6012        ",51000000,6.513%)
    ("dn_6005_6006        ",45000000,5.747%)
    ("dn_6001_6002        ",42000000,5.364%)
    ("dn_6029_6030        ",42000000,5.364%)
    ("dn_6031_6032        ",39000000,4.981%)
    ("dn_6035_6036        ",39000000,4.981%)
    ("dn_6009_6010        ",36000000,4.598%)
    ("dn_6003_6004        ",27000000,3.448%)
    ("dn_6033_6034        ",24000000,3.065%)
    ("dn_6019_6020        ",21000000,2.682%)
    ("dn_6025_6026        ",15000000,1.916%)
   (18 rows)

The table definition indicates that the table uses the **inv_date_sk** column as the distribution key, which causes a data skew. Based on the data distribution of each column, change the distribution key to **inv_item_sk**. The skewness is as follows:

.. code-block::

   postgres=# select table_skewness('inventory');
                 table_skewness
   ------------------------------------------
    ("dn_6001_6002        ",43934200,5.611%)
    ("dn_6007_6008        ",43829420,5.598%)
    ("dn_6003_6004        ",43781960,5.592%)
    ("dn_6031_6032        ",43773880,5.591%)
    ("dn_6033_6034        ",43763280,5.589%)
    ("dn_6011_6012        ",43683600,5.579%)
    ("dn_6013_6014        ",43551660,5.562%)
    ("dn_6027_6028        ",43546340,5.561%)
    ("dn_6009_6010        ",43508700,5.557%)
    ("dn_6023_6024        ",43484540,5.554%)
    ("dn_6019_6020        ",43466800,5.551%)
    ("dn_6021_6022        ",43458500,5.550%)
    ("dn_6017_6018        ",43448040,5.549%)
    ("dn_6015_6016        ",43247700,5.523%)
    ("dn_6005_6006        ",43200240,5.517%)
    ("dn_6029_6030        ",43181360,5.515%)
    ("dn_6025_6026        ",43179700,5.515%)
    ("dn_6035_6036        ",42960080,5.487%)
   (18 rows)

Data skew is solved.

Data Skew in the Computing Layer
--------------------------------

Even if data is balanced across nodes after you change the distribution key of a table, data skew may still occur during a query. If data skew occurs in the result set of an operator on a DN, skew will also occur during the computing that involves the operator. Generally, this is caused by data redistribution during the execution.

During a query, **JOIN** keys and **GROUP BY** keys are not used as distribution keys. Data is redistributed among DNs based on the hash values of data on the keys. The redistribution is implemented using the **Redistribute** operator in an execution plan. Data skew in redistribution columns can lead to data skew during system operation. After the redistribution, some nodes will have much more data, process more data, and will have much lower performance than others.

In the following example, the **s** and **t** tables are joined, and **s.x** and **t.x** columns in the join condition are not their distribution keys. Table data is redistributed using the **Redistribute** operator. Data skew occurs in the **s.x** column and not in the **t.x** column. The result set of the stream operator (**id** = **6**) on **datanode2** has data three times that of other DNs and causes a skew.

.. code-block::

   select * from skew s,test t where s.x = t.x order by s.a limit 1;

.. code-block::

    id |                      operation                      |        A-time
   ----+-----------------------------------------------------+-----------------------
     1 | ->  Limit                                           | 52622.382
     2 |    ->  Streaming (type: GATHER)                     | 52622.374
     3 |       ->  Limit                                     | [30138.494,52598.994]
     4 |          ->  Sort                                   | [30138.486,52598.986]
     5 |             ->  Hash Join (6,8)                     | [30127.013,41483.275]
     6 |                ->  Streaming(type: REDISTRIBUTE)    | [11365.110,22024.845]
     7 |                   ->  Seq Scan on public.skew s     | [2019.168,2175.369]
     8 |                ->  Hash                             | [2460.108,2499.850]
     9 |                   ->  Streaming(type: REDISTRIBUTE) | [1056.214,1121.887]
    10 |                      ->  Seq Scan on public.test t  | [310.848,325.569]
   (10 rows)
    6 --Streaming(type: REDISTRIBUTE)
            datanode1 (rows=5050368)
            datanode2 (rows=15276032)
            datanode3 (rows=5174272)
            datanode4 (rows=5219328)

Compared with storage skew, computing skew is more difficult to identify in advance. Therefore, the Runtime Load Balance Technology (RLBT) solution is proposed to solve the computing skew problem during running. The RLBT solution consists of two parts: calculation skew identification and calculation skew resolution. The details are as follows:

#. Detect data skew.

   The solution first checks whether skew data exists in redistribution columns used for computing. RLBT can detect data skew based on statistics, specified hints, or rules.

   -  Detection based on statistics

      Run the **ANALYZE** statement to collect statistics on tables. The optimizer will automatically identify skew data on redistribution keys based on the statistics and generate optimization plans for queries having potential skew. When the redistribution key has multiple columns, statistics information can be used for identification only when all columns belong to the same base table.

      The statistics information can only provide the skewness of the base table. When a column in the base table is skewed, other columns have filtering conditions, or after the join of other tables, the skewed data may still exist on the skewed column.

   -  Detection based on specified hints

      The intermediate results of complex queries are difficult to estimate based on statistics. In this case, you can specify hints to provide the skew information, based on which the optimizer optimizes queries. For details about the syntax of hints, see :ref:`Skew Hints <opengauss_opti_0061>`.

   -  Detection based on rules

      In a business intelligence (BI) system, a large number of SQL statements having outer joins (including left joins, right joins, and full joins) are generated, and many NULL values will be generated in empty columns that have no match for outer joins. If JOIN or GROUP BY operations are performed on the columns, data skew will occur. RLBT can automatically identify this scenario and generate an optimization plan for NULL value skew.

#. Solve computing skew.

   **Join** and **Aggregate** operators are optimized to solve skew.

   -  **Join** optimization

   Skew and non-skew data is separately processed. Details are as follows:

   a. When redistribution is required on both sides of a join:

      Use **PART_REDISTRIBUTE_PART_ROUNDROBIN** on the side with skew. Specifically, perform round-robin on skew data and redistribution on non-skew data.

      Use **PART_REDISTRIBUTE_PART_BROADCAST** on the side with no skew. Specifically, perform broadcast on skew data and redistribution on non-skew data.

   b. When redistribution is required on only one side of a join:

      Use **PART_REDISTRIBUTE_PART_ROUNDROBIN** on the side where redistribution is required.

      Use **PART_LOCAL_PART_BROADCAST** on the side where redistribution is not required. Specifically, perform broadcast on skew data and retain other data locally.

   c. When a table has NULL values padded:

      Use **PART_REDISTERIBUTE_PART_LOCAL** on the table. Specifically, retain the NULL values locally and perform redistribution on other data.

   In the example query, the **s.x** column contains skewed data and its value is **0**. The optimizer identifies the skew data in statistics and generates the following optimization plan:

   .. code-block::

       id |                                operation                                |        A-time
      ----+-------------------------------------------------------------------------+-----------------------
        1 | ->  Limit                                                               | 23642.049
        2 |    ->  Streaming (type: GATHER)                                         | 23642.041
        3 |       ->  Limit                                                         | [23310.768,23618.021]
        4 |          ->  Sort                                                       | [23310.761,23618.012]
        5 |             ->  Hash Join (6,8)                                         | [20898.341,21115.272]
        6 |                ->  Streaming(type: PART REDISTRIBUTE PART ROUNDROBIN)   | [7125.834,7472.111]
        7 |                   ->  Seq Scan on public.skew s                         | [1837.079,1911.025]
        8 |                ->  Hash                                                 | [2612.484,2640.572]
        9 |                   ->  Streaming(type: PART REDISTRIBUTE PART BROADCAST) | [1193.548,1297.894]
       10 |                      ->  Seq Scan on public.test t                      | [314.343,328.707]
      (10 rows)
         5 --Vector Hash Join (6,8)
               Hash Cond: s.x = t.x
               Skew Join Optimizated by Statistic
         6 --Streaming(type: PART REDISTRIBUTE PART ROUNDROBIN)
               datanode1 (rows=7635968)
               datanode2 (rows=7517184)
               datanode3 (rows=7748608)
               datanode4 (rows=7818240)

   **Skew Join Optimizated by Statistic** indicates that the preceding execution plan is an optimized plan used for handling data skew. The **Statistic** keyword indicates that the plan optimization is based on statistics; **Hint** indicates that the optimization is based on hints; **Rule** indicates that the optimization is based on rules. In this plan, skew data and non-skew data are separately processed. Non-skew data in the **s** table is redistributed based on its hash values, and skew data (whose value is **0**) is evenly distributed on all nodes in round-robin mode. In this way, data skew is solved.

   To ensure result correctness, the **t** table also needs to be processed. In the **t** table, the data whose value is **0** (skew value in the **s.x** table) is broadcast and other data is redistributed based on its hash values.

   In this way, data skew in **JOIN** operations is solved. The above result shows that the output of the stream operator (**id** = **6**) is balanced and the end-to-end performance of the query is doubled.

   -  **Aggregate** optimization

   For aggregation, data on each DN is deduplicated based on the **GROUP BY** key and then redistributed. After the deduplication on DNs, the global occurrences of each value will not be greater than the number of DNs. Therefore, no serious data skew will occur. Take the following query as an example:

   **select c1, c2, c3, c4, c5, c6, c7, c8, c9, count(*) from t group by c1, c2, c3, c4, c5, c6, c7, c8, c9 limit 10;**

   The command output is as follows:

   .. code-block::

       id |                 operation                  |         A-time         |  A-rows
      ----+--------------------------------------------+------------------------+----------
        1 | ->  Streaming (type: GATHER)               | 130621.783             |       12
        2 |    ->  GroupAggregate                      | [85499.711,130432.341] |       12
        3 |       ->  Sort                             | [85499.509,103145.632] | 36679237
        4 |          ->  Streaming(type: REDISTRIBUTE) | [25668.897,85499.050]  | 36679237
        5 |             ->  Seq Scan on public.t       | [9835.069,10416.388]   | 36679237
      (5 rows)

         4 --Streaming(type: REDISTRIBUTE)
               datanode1 (rows=36678837)
               datanode2 (rows=100)
               datanode3 (rows=100)
               datanode4 (rows=200)

   A large amount of skew data exists. As a result, after data is redistributed based on its **GROUP BY** key, the data volume of datanode1 is hundreds of thousands of times that of others. After optimization, a GROUP BY operation is performed on the DN to deduplicate data. After redistribution, no data skew occurs.

   .. code-block::

       id |                 operation                  |        A-time
      ----+--------------------------------------------+-----------------------
        1 | ->  Streaming (type: GATHER)               | 10961.337
        2 |    ->  HashAggregate                       | [10953.014,10953.705]
        3 |       ->  HashAggregate                    | [10952.957,10953.632]
        4 |          ->  Streaming(type: REDISTRIBUTE) | [10952.859,10953.502]
        5 |             ->  HashAggregate              | [10084.280,10947.139]
        6 |                ->  Seq Scan on public.t    | [4757.031,5201.168]
      (6 rows)

       Predicate Information (identified by plan id)
      -----------------------------------------------
         3 --HashAggregate
               Skew Agg Optimized by Statistic
      (2 rows)

         4 --Streaming(type: REDISTRIBUTE)
               datanode1 (rows=17)
               datanode2 (rows=8)
               datanode3 (rows=8)
               datanode4 (rows=14)

   Applicable scope

   -  **Join** operators

      -  **nest loop**, **merge join**, and **hash join** are supported.
      -  If skew data is on the left to the join, **inner join**, **left join**, **semi join**, and **anti join** are supported. If skew data is on the right to the join, **inner join**, **right join**, **right semi join**, and **right anti join** are supported.
      -  For an optimization plan generated based on statistics, the optimizer checks whether it is optimal by estimating its cost. Optimization plans based on hints or rules are forcibly generated.

   -  **Aggregate** operators

      -  **array_agg**, **string_agg**, and **subplan in agg qual** cannot be optimized.
      -  A plan generated based on statistics is affected by its cost, the **plan_mode_seed** parameter, and the **best_agg_plan** parameter. A plan generated based on hints or rules are not affected by them.
