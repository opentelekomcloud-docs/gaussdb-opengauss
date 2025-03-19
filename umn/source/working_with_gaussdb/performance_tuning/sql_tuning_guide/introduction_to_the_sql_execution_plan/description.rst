:original_name: opengauss_opti_0033.html

.. _opengauss_opti_0033:

Description
===========

As described in :ref:`Overview <opengauss_opti_0032>`, **EXPLAIN** displays execution plans, but will not actually run SQL statements. **EXPLAIN ANALYZE** and **EXPLAIN PERFORMANCE** both will actually run SQL statements and return the execution information. This section describes execution plans and execution information in detail.

.. _en-us_topic_0000002124197009__section14981mcpsimp:

Execution Plans
---------------

The following SQL statements are used as an example.

.. code-block::

   select
       cjxh,
       count(1)
   from dwcjk
   group by cjxh;

Run the **EXPLAIN** command and the output is as follows.

|image1|

**Interpretation of the execution plan fields (horizontal)**:

-  **id**: execution operator node ID

-  **operation**: name of an execution operator

   An operator prefixed with **Vector** is a vectorized executor operator, usually used in a query containing a column-store table.

   Streaming is a special operator. It has three types, which correspond to different data shuffle functions in the distributed architecture.

   -  Streaming (type: GATHER): A CN collects data from DNs.
   -  Streaming (type: REDISTRIBUTE): Data is redistributed to all the DNs based on selected columns.
   -  Streaming (type: BROADCAST): Data on the current DN is broadcast to other DNs.

-  **E-rows**: number of output rows estimated by each operator

-  **E-memory**: estimated memory used by each operator on a DN. Only operators executed on DNs are displayed. In certain scenarios, the memory upper limit enclosed in parentheses will be displayed following the estimated memory usage.

-  **E-width**: estimated width of an output tuple of each operator

-  **E-costs**: execution cost estimated by each operator

   -  **E-costs** is measured by the optimizer based on an overhead unit. Usually, fetching a disk page is defined as a unit. Other overhead parameters are configured based on the unit.
   -  The overhead of each node (specified by **E-costs**) includes the overheads of all its child nodes.
   -  Such an overhead reflects only what the optimizer is concerned about, but does not consider the time for transferring result rows to the client. Although the time may play an important role in the actual total time, it is ignored by the optimizer because it cannot be changed by modifying the plan.

**Interpretation of the execution plan layers (vertical)**:

#. Layer 1: **CStore Scan on dwcjk**

   The table scan operator scans the table **dwcjk** using **CStore Scan**. At this layer, data in the table **dwcjk** is read from a buffer or disk, and then transferred to the upper-layer node for calculation.

#. Layer 2: **Vector Hash Aggregate**

   Aggregation operator is used to aggregate (**GROUP BY**) data transferred from the lower layer.

#. Layer 3: **Vector Streaming (type: GATHER)**

   The GATHER-typed Shuffle operator aggregates data from DNs to the CN.

#. Layer 4: **Row Adapter**

   Storage format conversion operator is used to convert memory data from column storage to row storage for client display.

If the operator in the top layer is **Data Node Scan**, set **enable_fast_query_shipping** to **off** to view the detailed execution plan.

.. code-block::

   postgres=#  explain select cjxh, count(1) from dwcjk group by cjxh;
                       QUERY PLAN
   --------------------------------------------------
    Data Node Scan  (cost=0.00..0.00 rows=0 width=0)
      Node/s: All datanodes
   (2 rows)

The execution plan will be displayed as follows.

|image2|

**Keywords in the execution plan**:

#. Table access modes

   -  Seq Scan

      Scans all rows of the table in sequence.

   -  Index Scan

      The optimizer uses a two-step plan: the lower-layer plan node visits an index to find the locations of rows matching the index condition, and then the upper-layer plan node actually fetches those rows from the table itself. Fetching rows separately is much more expensive than reading them sequentially, but because not all pages of the table have to be visited, this is still cheaper than a sequential scan. The upper-layer plan node sorts index-identified rows based on their physical locations before reading them. This minimizes the independent capturing overhead.

      If there are separate indexes on multiple columns referenced in **WHERE**, the optimizer might use an **AND** or **OR** combination of the indexes. However, this requires the visiting of both indexes, so it is not necessarily a win compared with the method using just one index and treating the other condition as a filter.

      Index scans are featured with different sorting mechanisms and can be classified into the following types.

      -  Bitmap index scan

         Fetches data pages using a bitmap.

      -  Index scan using **index_name**

         Fetches table rows in index order, which makes them even more expensive to read. However, there are so few rows that the extra cost of sorting the row locations is unnecessary. This plan type is used mainly for queries fetching just a single row and queries having an **ORDER BY** condition that matches the index order, because no extra sorting step is needed to satisfy **ORDER BY**.

#. Table connection modes

   -  Nested Loop

      A nested loop is used for queries that have a smaller data set connected. In a nested loop, the foreign table drives the internal table and each row returned from the foreign table should have a matching row in the internal table. The returned result set of all queries should be less than 10,000. The table that returns a smaller subset will work as a foreign table, and indexes are recommended for connection columns of the internal table.

   -  (Sonic) Hash Join

      A hash join is used for large tables. The optimizer uses the join key and the smaller table among the two tables to build a hash table in memory, and then scans the larger table and probes the hash to find the rows that match the hash. Sonic and non-Sonic hash joins differ in their hash table structures, which do not affect the execution result set.

   -  Merge Join

      In most cases, merge join is inferior to hash join in execution performance. If the source data has been sorted, the data does not need to be sorted again when the merge join is performed. In this case, the performance of the merge join is better than that of the hash join.

#. Operators

   -  SORT

      Sorts the result set.

   -  FILTER

      The EXPLAIN output shows that the **WHERE** clause is attached to the sequential scan plan node as a **Filter** condition. This means that the plan node checks the condition for each row it scans and outputs only the rows that meet the condition. The estimated number of output rows has been reduced because of the **WHERE** clause. However, the scan will still have to visit all 10,000 rows, as a result, the cost is not decreased. It increases a bit (by 10,000 x **cpu_operator_cost**) to reflect the extra CPU time spent on checking the **WHERE** condition.

   -  LIMIT

      Limits the number of output execution results. If a **LIMIT** condition is added, not all rows are retrieved.

Task Execution
--------------

In SQL optimization process, you can use **EXPLAIN ANALYZE** or **EXPLAIN PERFORMANCE** to check the SQL statement execution information. By comparing estimation differences between actual implementation and the optimizer, basis for service optimization is provided. **EXPLAIN PERFORMANCE** provides the execution information on each DN, whereas **EXPLAIN ANALYZE** does not.

The following SQL statement is used as an example:

.. code-block::

   select count(1) from tb1;

The output of running **EXPLAIN PERFORMANCE** is as follows:

|image3|

|image4|

|image5|

|image6|

In the above figures, the execution information is classified into the following seven parts.

#. The plan is displayed as a table, which contains 11 columns: **id**, **operation**, **A-time**, **A-rows**, **E-rows**, **E-distinct**, **Peak Memory**, **E-memory**, **A-width**, **E-width**, and **E-costs**. The definition of the plan-type columns (columns started with **id**, **operation**, or **E**) is the same as that of running **EXPLAIN**. For details, see :ref:`Execution Plans <en-us_topic_0000002124197009__section14981mcpsimp>`. The definition of **A-time**, **A-rows**, **E-distinct**, **Peak Memory**, and **A-width** are as follows:

   -  **A-time**: execution completion time of the current operator. Generally, the A-time of the operator executed on DNs is two values enclosed by square brackets ([]), indicating the shortest time and longest time for completing the operator on all DNs, respectively.
   -  **A-rows**: number of actual output tuples of the operator
   -  **E-distinct**: estimated distinct value of the hash join operator
   -  **Peak Memory**: peak memory of the operator on each DN
   -  **A-width**: actual tuple width in each row of the current operator. This parameter is valid only for heavy memory operators, including **(Vec)HashJoin**, **(Vec)HashAgg**, **(Vec)HashSetOp**, **(Vec)Sort**, and **(Vec)Materialize**. The **(Vec)HashJoin** calculation width is the width of its right subtree operator and will be displayed on the right subtree.

#. **Predicate Information (identified by plan id)**:

   This part displays the static information that does not change in the plan execution process, such as some join conditions and filter information.

#. **Memory Information (identified by plan id)**:

   This part displays the memory usage information printed by certain operators (mainly Hash and Sort), including **peak memory**, **control memory**, **operator memory**, **width**, **auto spread num**, and **early spilled**; and spill details, including **spill Time(s)**, **inner/outer partition spill num**, **temp file num**, spilled data volume, and **written disk IO [**\ *min, max*\ **]**.

#. **Targetlist Information (identified by plan id)**:

   This part displays the target columns provided by each operator.

#. **DataNode Information (identified by plan id)**:

   The execution time, CPU, and buffer usage of each operator are printed in this part.

#. **User Define Profiling**:

   This part displays CNs and DNs, DN and DN connection time, and some execution information in the storage layer.

#. **====== Query Summary =====**:

   The total execution time and network traffic, including the maximum and minimum execution time in the initialization and end phases on each DN, initialization, execution, and time in the end phase on each CN, and the system available memory during the current statement execution, and statement estimation memory information.

   .. important::

      -  The difference between **A-rows** and **E-rows** shows the deviation between the optimizer estimation and actual execution. Generally, if the deviation is larger, the plan generated by the optimizer is more improper, and more manual intervention and optimization are required.
      -  If the difference of the **A-time** values is large, it indicates that the operator computing skew (difference between execution time on DNs) is large and that manual performance tuning is required.
      -  **Max Query Peak Memory** is often used to estimate the consumed memory of SQL statements, and is also used as an important basis for configuring a running memory parameter during SQL tuning. Generally, the output from **EXPLAIN ANALYZE** or **EXPLAIN PERFORMANCE** is provided for the input for further tuning.

.. |image1| image:: /_static/images/en-us_image_0000002088518150.png
.. |image2| image:: /_static/images/en-us_image_0000002088518142.png
.. |image3| image:: /_static/images/en-us_image_0000002124277733.png
.. |image4| image:: /_static/images/en-us_image_0000002124277725.png
.. |image5| image:: /_static/images/en-us_image_0000002088677998.png
.. |image6| image:: /_static/images/en-us_image_0000002124197441.png
