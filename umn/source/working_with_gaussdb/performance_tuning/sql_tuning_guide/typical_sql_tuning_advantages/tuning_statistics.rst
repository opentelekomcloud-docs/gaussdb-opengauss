:original_name: opengauss_opti_0048.html

.. _opengauss_opti_0048:

Tuning Statistics
=================

Background
----------

GaussDB generates optimal execution plans based on the cost estimation. Optimizers need to estimate the number of data rows and the cost based on statistics collected using **ANALYZE**. Therefore, the statistics is vital for the estimation of the number of rows and cost. Global statistics collected using **ANALYZE** include: **relpages** and **reltuples** in the **pg_class** table; **stadistinct**, **stanullfrac**, **stanumbersN**, **stavaluesN**, and **histogram_bounds** in the **pg_statistic** table.

Example 1: Poor Query Performance Due to the Lack of Statistics
---------------------------------------------------------------

In most cases, the lack of statistics in tables or columns involved in the query greatly affects the query performance.

The table structure is as follows:

.. code-block::

   CREATE TABLE LINEITEM
   (
   L_ORDERKEY         BIGINT        NOT NULL
   , L_PARTKEY        BIGINT        NOT NULL
   , L_SUPPKEY        BIGINT        NOT NULL
   , L_LINENUMBER     BIGINT        NOT NULL
   , L_QUANTITY       DECIMAL(15,2) NOT NULL
   , L_EXTENDEDPRICE  DECIMAL(15,2) NOT NULL
   , L_DISCOUNT       DECIMAL(15,2) NOT NULL
   , L_TAX            DECIMAL(15,2) NOT NULL
   , L_RETURNFLAG     CHAR(1)       NOT NULL
   , L_LINESTATUS     CHAR(1)       NOT NULL
   , L_SHIPDATE       DATE          NOT NULL
   , L_COMMITDATE     DATE          NOT NULL
   , L_RECEIPTDATE    DATE          NOT NULL
   , L_SHIPINSTRUCT   CHAR(25)      NOT NULL
   , L_SHIPMODE       CHAR(10)      NOT NULL
   , L_COMMENT        VARCHAR(44)   NOT NULL
   ) with (orientation = column, COMPRESSION = MIDDLE) distribute by hash(L_ORDERKEY);

   CREATE TABLE ORDERS
   (
   O_ORDERKEY        BIGINT        NOT NULL
   , O_CUSTKEY       BIGINT        NOT NULL
   , O_ORDERSTATUS   CHAR(1)       NOT NULL
   , O_TOTALPRICE    DECIMAL(15,2) NOT NULL
   , O_ORDERDATE     DATE NOT NULL
   , O_ORDERPRIORITY CHAR(15)      NOT NULL
   , O_CLERK         CHAR(15)      NOT NULL
   , O_SHIPPRIORITY  BIGINT        NOT NULL
   , O_COMMENT       VARCHAR(79)   NOT NULL
   )with (orientation = column, COMPRESSION = MIDDLE) distribute by hash(O_ORDERKEY);

The query statements are as follows:

.. code-block::

   explain verbose select
   count(*) as numwait
   from
   lineitem l1,
   orders
   where
   o_orderkey = l1.l_orderkey
   and o_orderstatus = 'F'
   and l1.l_receiptdate > l1.l_commitdate
   and not exists (
   select
   *
   from
   lineitem l3
   where
   l3.l_orderkey = l1.l_orderkey
   and l3.l_suppkey <> l1.l_suppkey
   and l3.l_receiptdate > l3.l_commitdate
   )
   order by
   numwait desc;

If such an issue occurs, you can use the following methods to check whether statistics in tables or columns has been collected using **ANALYZE**.

#. Execute **EXPLAIN VERBOSE** in the query to analyze the execution plan and check the warning information:

   .. code-block::

      WARNING:Statistics in some tables or columns(public.lineitem.l_receiptdate, public.lineitem.l_commitdate, public.lineitem.l_orderkey, public.lineitem.l_suppkey, public.orders.o_orderstatus, public.orders.o_orderkey) are not collected.
      HINT:Do analyze for them in order to generate optimized plan.

#. Check whether the following information exists in the log file in the **pg_log** directory. If it does, the poor query performance was caused by the lack of statistics in some tables or columns.

   .. code-block::

      2017-06-14 17:28:30.336 CST 140644024579856 20971684 [BACKEND] LOG:Statistics in some tables or columns(public.lineitem.l_receiptdate, public.lineitem.l_commitdate, public.lineitem.l_orderkey, public.linei
      tem.l_suppkey, public.orders.o_orderstatus, public.orders.o_orderkey) are not collected.
      2017-06-14 17:28:30.336 CST 140644024579856 20971684 [BACKEND] HINT:Do analyze for them in order to generate optimized plan.

By using any of the preceding methods, you can identify tables or columns whose statistics have not been collected using **ANALYZE**. You can execute **ANALYZE** to warnings or tables and columns recorded in logs to resolve the problem.

Example 3: Tuning is Not Accurate When Intermediate Results Exist in the Query Where JOIN Is Used for Multiple Tables
---------------------------------------------------------------------------------------------------------------------

**Symptom**: Query the personnel who have registered in an Internet cafe within 15 minutes before and after the registration of a specified person.

.. code-block::

   SELECT
   C.WBM,
   C.DZQH,
   C.DZ,
   B.ZJHM,
   B.SWKSSJ,
   B.XWSJ
   FROM
   b_zyk_wbswxx A,
   b_zyk_wbswxx B,
   b_zyk_wbcs C
   WHERE
   A.ZJHM = '522522******3824'
   AND A.WBDM = B.WBDM
   AND A.WBDM = C.WBDM
   AND abs(to_date(A.SWKSSJ,'yyyymmddHH24MISS') - to_date(B.SWKSSJ,'yyyymmddHH24MISS')) < INTERVAL '15 MINUTES'
   ORDER BY
   B.SWKSSJ,
   B.ZJHM
   limit 10 offset 0
   ;

:ref:`Figure 1 <en-us_topic_0000002124277453___d0e39484>` shows the execution plan. This query takes about 12 seconds.

.. _en-us_topic_0000002124277453___d0e39484:

.. figure:: /_static/images/en-us_image_0000002124278009.png
   :alt: **Figure 1** Using an unlogged table (1)

   **Figure 1** Using an unlogged table (1)

**Tuning analysis:**

#. In the execution plan, index scan is used for node scanning, the **Join Filter** calculation in the external **NEST LOOP JOIN** statement consumes most of the query time, and the calculation uses the string addition and subtraction, and unequal-value comparison.

#. Use an unlogged table to record the Internet access time of the specified person. The start time and end time are processed during data insertion, and this reduces subsequent addition and subtraction operations.

   ::

      // Create a temporary unlogged table.
      CREATE UNLOGGED TABLE temp_tsw
      (
      ZJHM         NVARCHAR2(18),
      WBDM         NVARCHAR2(14),
      SWKSSJ_START NVARCHAR2(14),
      SWKSSJ_END   NVARCHAR2(14),
      WBM          NVARCHAR2(70),
      DZQH         NVARCHAR2(6),
      DZ           NVARCHAR2(70),
      IPDZ         NVARCHAR2(39)
      )
      ;
      // Insert the Internet access record of the specified person, and process the start time and end time.
      INSERT INTO
      temp_tsw
      SELECT
      A.ZJHM,
      A.WBDM,
      to_char((to_date(A.SWKSSJ,'yyyymmddHH24MISS') - INTERVAL '15 MINUTES'),'yyyymmddHH24MISS'),
      to_char((to_date(A.SWKSSJ,'yyyymmddHH24MISS') + INTERVAL '15 MINUTES'),'yyyymmddHH24MISS'),
      B.WBM,B.DZQH,B.DZ,B.IPDZ
      FROM
      b_zyk_wbswxx A,
      b_zyk_wbcs B
      WHERE
      A.ZJHM='522522******3824' AND A.WBDM = B.WBDM
      ;

      // Query the personnel who have registered in an Internet cafe before and after 15 minutes of the registration of the specified person. Convert their ID card number format to int8 in comparison.
      SELECT
      A.WBM,
      A.DZQH,
      A.DZ,
      A.IPDZ,
      B.ZJHM,
      B.XM,
      to_date(B.SWKSSJ,'yyyymmddHH24MISS') as SWKSSJ,
      to_date(B.XWSJ,'yyyymmddHH24MISS') as XWSJ,
      B.SWZDH
      FROM temp_tsw A,
      b_zyk_wbswxx B
      WHERE
      A.ZJHM <> B.ZJHM
      AND A.WBDM = B.WBDM
      AND (B.SWKSSJ)::int8 > (A.swkssj_start)::int8
      AND (B.SWKSSJ)::int8 < (A.swkssj_end)::int8
      order by
      B.SWKSSJ,
      B.ZJHM
      limit 10 offset 0
      ;

   The query takes about 7s. :ref:`Figure 2 <en-us_topic_0000002124277453___d0e39505>` shows the execution plan.

   .. _en-us_topic_0000002124277453___d0e39505:

   .. figure:: /_static/images/en-us_image_0000002124278005.png
      :alt: **Figure 2** Using an unlogged table (2)

      **Figure 2** Using an unlogged table (2)

#. In the previous plan, **Hash Join** has been executed, and a Hash table has been created for the large table **b_zyk_wbswxx**. The table contains large amounts of data, so the creation takes long time.

   **temp_tsw** contains only hundreds of records, and an equal-value connection is created between **temp_tsw** and **b_zyk_wbswxx** using wbdm (an Internet cafe code). Therefore, if **JOIN** is changed to **NEST LOOP JOIN**, index scan can be used for node scanning, and the performance will be pulled up.

#. Execute the following statement to change **JOIN** to **NEST LOOP JOIN**.

   .. code-block::

      SET enable_hashjoin = off;

   :ref:`Figure 3 <en-us_topic_0000002124277453___d0e39522>` shows the execution plan. The query takes about 3s.

   .. _en-us_topic_0000002124277453___d0e39522:

   .. figure:: /_static/images/en-us_image_0000002088518422.png
      :alt: **Figure 3** Using an unlogged table (3)

      **Figure 3** Using an unlogged table (3)

#. Save the query result set in the unlogged table for paging display.

   If paging display needs to be achieved on the upper-layer application page, change the **offset** value to determine the result set on the target page. In this way, the previous query statement will be executed every time after a page turning operation, which causes long response latency.

   To resolve this problem, the unlogged table is recommended to save the result set.

   ::

      // Create an unlogged table to save the result set.
      CREATE UNLOGGED TABLE temp_result
      (
      WBM      NVARCHAR2(70),
      DZQH     NVARCHAR2(6),
      DZ       NVARCHAR2(70),
      IPDZ     NVARCHAR2(39),
      ZJHM     NVARCHAR2(18),
      XM       NVARCHAR2(30),
      SWKSSJ   date,
      XWSJ     date,
      SWZDH    NVARCHAR2(32)
      );

      // Insert the result set to the unlogged table. It takes about 3s.
      INSERT INTO
      temp_result
      SELECT
      A.WBM,
      A.DZQH,
      A.DZ,
      A.IPDZ,
      B.ZJHM,
      B.XM,
      to_date(B.SWKSSJ,'yyyymmddHH24MISS') as SWKSSJ,
      to_date(B.XWSJ,'yyyymmddHH24MISS') as XWSJ,
      B.SWZDH
      FROM temp_tsw A,
      b_zyk_wbswxx B
      WHERE
      A.ZJHM <> B.ZJHM
      AND A.WBDM = B.WBDM
      AND (B.SWKSSJ)::int8 > (A.swkssj_start)::int8
      AND (B.SWKSSJ)::int8 < (A.swkssj_end)::int8
      ;

      // Perform paging query on the result set. It takes about 10 ms.
      SELECT
      *
      FROM
      temp_result
      ORDER BY
      SWKSSJ,
      ZJHM
      LIMIT 10 OFFSET 0;

.. caution::

   Collecting more accurate statistics usually improves the query performance, but may also deteriorate the performance. If the performance deteriorates, you can:

-  Restore the default statistics.
-  Use hints to force the optimizer to use the optimal query plan. (For details, see :ref:`Hint-based Tuning <opengauss_opti_0053>`.)
