:original_name: opengauss_opti_0032.html

.. _opengauss_opti_0032:

Overview
========

A SQL execution plan is a node tree, which displays detailed procedure when GaussDB runs a SQL statement. A database operator indicates one step.

You can run the **EXPLAIN** command to view execution plans generated for each query by an optimizer. The output of **EXPLAIN** has one row for each execution node, showing the basic node type and the cost estimation that the optimizer made for the execution of this node. :ref:`Figure 1 <en-us_topic_0000002088517706___d0e37190>` shows an example.

.. _en-us_topic_0000002088517706___d0e37190:

.. figure:: /_static/images/en-us_image_0000002124277597.jpg
   :alt: **Figure 1** SQL execution plan example

   **Figure 1** SQL execution plan example

-  The bottom-layer node is the table scan node, which scans the table and returns the original data rows. The types of scan nodes (sequential scans and index scans) vary depending on the table access methods. The scan object of the bottom-layer node may also be non-table row data (that is not directly read from the table), such as the VALUES clause and the function that returns a row set. They have their own scan node types.
-  If a query requires joining, aggregation, sorting, or other operations on the original row, other nodes are added to the scan node. In addition, there is more than one way to perform these operations, so different types of execution nodes may be displayed.
-  The first row (the upper-layer node) estimates the total execution cost of the execution plan. Such an estimate indicates the value that the optimizer tries to minimize.

Execution Plan Display Format
-----------------------------

GaussDB provides four display formats: **normal**, **pretty**, **summary**, and **run**.

-  **normal** indicates that the default printing format is used, as shown in :ref:`Figure 1 <en-us_topic_0000002088517706___d0e37190>`.
-  **pretty** indicates the optimized display format of GaussDB is used. The new format contains a plan node ID, directly and effectively analyzing performance, as shown in :ref:`Figure 2 <en-us_topic_0000002088517706___d0e37226>`.
-  **summary** indicates that analysis of the **pretty** printed information is added.
-  **run** indicates that the system exports the printed information specified by **summary** as a CSV file for further analysis.

.. _en-us_topic_0000002088517706___d0e37226:

.. figure:: /_static/images/en-us_image_0000002124197301.png
   :alt: **Figure 2** Example of an execution plan using the pretty format

   **Figure 2** Example of an execution plan using the pretty format

You can change the display formats of execution plans by configuring **explain_perf_mode**. Later examples use the pretty format by default.

Execution Plan Information
--------------------------

In addition to configuring different display formats for execution plans, you can use different **EXPLAIN** syntax to display execution plan information in detail.

-  EXPLAIN *statement*: only generates an execution plan and does not execute. The *statement* indicates SQL statements.
-  EXPLAIN ANALYZE *statement*: generates and executes an execution plan, and displays the execution summary. Then actual execution time statistics are added, including the total elapsed time expended within each plan node (in milliseconds) and the total number of rows it actually returned.
-  EXPLAIN PERFORMANCE *statement*: generates and executes an execution plan, and displays all execution information.

To measure the run time cost of each node in the execution plan, the current execution of **EXPLAIN ANALYZE** or **EXPLAIN PERFORMANCE** adds profiling overhead to query execution. Running **EXPLAIN ANALYZE** or **PERFORMANCE** on a query sometimes takes longer time than executing the query normally. The amount of overhead depends on the nature of the query, as well as the platform being used. For details, see "SQL Reference" > "SQL Syntax" > "EXPLAIN" in *GaussDB Development Guide (1.x)*.

Therefore, if a SQL statement is not finished after being running for a long time, run the **EXPLAIN** statement to view the execution plan and then locate the fault. If the SQL statement has been properly executed, run the **EXPLAIN ANALYZE** or **EXPLAIN PERFORMANCE** statement to check the execution plan and information to locate the fault.

The **EXPLAIN PERFORMANCE** lightweight execution is consistent with **EXPLAIN PERFORMANCE** but greatly reduces the time spent on performance analysis.
