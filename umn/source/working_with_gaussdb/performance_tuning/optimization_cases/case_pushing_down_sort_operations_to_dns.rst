:original_name: opengauss_opti_0069.html

.. _opengauss_opti_0069:

Case: Pushing Down Sort Operations to DNs
=========================================

Symptom
-------

In an execution plan, more than 95% of the execution time is spent on **window agg** performed on the CN. In this case, **sum** is performed for the two columns separately, and then another **sum** is performed for the separate sum results of the two columns. After this, **trunc** and **sort** are performed in sequence.

The table structure is as follows:

::

   CREATE TABLE public.test(imsi int,L4_DW_THROUGHPUT int,L4_UL_THROUGHPUT int)
   with (orientation = column) DISTRIBUTE BY hash(imsi);

The query statements are as follows:

::

   SELECT COUNT(1) over() AS DATACNT,
   IMSI AS IMSI_IMSI,
   CAST(TRUNC(((SUM(L4_UL_THROUGHPUT) + SUM(L4_DW_THROUGHPUT))), 0) AS
   DECIMAL(20)) AS TOTAL_VOLOME_KPIID
   FROM public.test AS test
   GROUP BY IMSI
   order by TOTAL_VOLOME_KPIID DESC;

The execution plan is as follows:

::

   Row Adapter  (cost=10.70..10.70 rows=10 width=12)
      ->  Vector Sort  (cost=10.68..10.70 rows=10 width=12)
            Sort Key: ((trunc((((sum(l4_ul_throughput)) + (sum(l4_dw_throughput))))::numeric, 0))::numeric(20,0))
            ->  Vector WindowAgg  (cost=10.09..10.51 rows=10 width=12)
                  ->  Vector Streaming (type: GATHER)  (cost=242.04..246.84 rows=240 width=12)
                        Node/s: All datanodes
                        ->  Vector Hash Aggregate  (cost=10.09..10.29 rows=10 width=12)
                              Group By Key: imsi
                              ->  CStore Scan on test  (cost=0.00..10.01 rows=10 width=12)

Both **window agg** and **sort** are performed on the CN, which is time consuming.

Optimization Analysis
---------------------

Modify the statement to a subquery statement, as shown below:

::

   SELECT COUNT(1) over() AS DATACNT, IMSI_IMSI, TOTAL_VOLOME_KPIID
   FROM (SELECT IMSI AS IMSI_IMSI,
   CAST(TRUNC(((SUM(L4_UL_THROUGHPUT) + SUM(L4_DW_THROUGHPUT))),
   0) AS DECIMAL(20)) AS TOTAL_VOLOME_KPIID
   FROM public.test AS test
   GROUP BY IMSI
   ORDER BY TOTAL_VOLOME_KPIID DESC);

Perform **sum** on the **trunc** results of the two columns, take it as a subquery, and then perform **window agg** for the subquery to push down the sorting operation, as shown below:

::

   Row Adapter  (cost=10.70..10.70 rows=10 width=24)
      ->  Vector WindowAgg  (cost=10.45..10.70 rows=10 width=24)
            ->  Vector Streaming (type: GATHER)  (cost=250.83..253.83 rows=240 width=24)
                  Node/s: All datanodes
                  ->  Vector Sort  (cost=10.45..10.48 rows=10 width=12)
                        Sort Key: ((trunc(((sum(test.l4_ul_throughput) + sum(test.l4_dw_throughput)))::numeric, 0))::numeric(20,0))
                        ->  Vector Hash Aggregate  (cost=10.09..10.29 rows=10 width=12)
                              Group By Key: test.imsi
                              ->  CStore Scan on test  (cost=0.00..10.01 rows=10 width=12)

The optimized SQL statement greatly improves the performance by reducing the execution time from 120s to 7s.
