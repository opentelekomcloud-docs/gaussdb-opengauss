:original_name: opengauss_opti_0030.html

.. _opengauss_opti_0030:

Query Execution Process
=======================

:ref:`Figure 1 <en-us_topic_0000002088677406__fig203688151768>` and :ref:`Table 1 <en-us_topic_0000002088677406___d0e37026>` show the process from receiving SQL statements to the statement execution by the SQL engine. The texts in red are steps where database administrators optimize queries.

.. _en-us_topic_0000002088677406__fig203688151768:

.. figure:: /_static/images/en-us_image_0000002124197229.png
   :alt: **Figure 1** Execution process of query-related SQL statements by the SQL engine

   **Figure 1** Execution process of query-related SQL statements by the SQL engine

.. _en-us_topic_0000002088677406___d0e37026:

.. table:: **Table 1** Execution process of query-related SQL statements by the SQL engine

   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Procedure                          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
   +====================================+==============================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | Perform syntax and lexical parsing | Converts the input SQL statements from the string data type to the formatted structure stmt based on the specified SQL statement rules.                                                                                                                                                                                                                                                                                                                                      |
   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Perform semantic parsing           | Converts the formatted structure obtained from the previous step into objects that can be recognized by the database.                                                                                                                                                                                                                                                                                                                                                        |
   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Rewrite the query                  | Converts the output of the previous step into the structure that optimizes the query execution.                                                                                                                                                                                                                                                                                                                                                                              |
   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Optimize the query                 | Determines the execution mode of SQL statements (the execution plan) based on the result obtained from the last step and the internal database statistics. For details about how the statistics and GUC parameters affect the query optimization (execution plan), see :ref:`Optimizing Queries Using Statistics <en-us_topic_0000002088677406__section14925mcpsimp>` and :ref:`Optimizing Queries Using GUC Parameters <en-us_topic_0000002088677406__section14927mcpsimp>` |
   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Perform the query                  | Executes the SQL statements based on the execution path specified in the previous step. Selecting a proper underlying storage mode improves the query execution efficiency. For details, see :ref:`Optimizing Queries Using the Underlying Storage <en-us_topic_0000002088677406__section14933mcpsimp>`.                                                                                                                                                                     |
   +------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000002088677406__section14925mcpsimp:

Optimizing Queries Using Statistics
-----------------------------------

The GaussDB optimizer is a typical cost-based optimization (CBO). By using CBO, the database calculates the number of tuples and the execution cost for each step under each execution plan based on the number of table tuples, column width, null record ratio, and characteristic values, such as distinct, MCV, and HB values, and certain cost calculation methods. The database then selects the execution plan that takes the lowest cost for the overall execution or for the return of the first tuple. These characteristic values are the statistics, which is the core for optimizing a query. Accurate statistics helps the planner select the most appropriate query plan. Generally, you can collect statistics of a table or that of some columns in a table using **ANALYZE**. You are advised to periodically execute **ANALYZE** or execute it immediately after you modified most contents in a table.

.. _en-us_topic_0000002088677406__section14927mcpsimp:

Optimizing Queries Using GUC Parameters
---------------------------------------

Optimizing queries aims to select an efficient execution mode.

Take the following SQL statement as an example:

.. code-block::

   select count(1)
   from customer inner join store_sales on (ss_customer_sk = c_customer_sk);

During execution of **customer inner join store_sales**, GaussDB supports nested loop, merge join, and hash join. The optimizer estimates the result set value and the execution cost under each join mode based on the statistics of the **customer** and **store_sales** tables and selects the execution plan that takes the lowest execution cost.

As described in the preceding content, the execution cost is calculated based on certain methods and statistics. If the actual execution cost cannot be accurately estimated, you need to optimize the execution plan by configuring the GUC parameters.

.. _en-us_topic_0000002088677406__section14933mcpsimp:

Optimizing Queries Using the Underlying Storage
-----------------------------------------------

GaussDB supports row- and column-store tables. The selection of underlying storage strongly depends on specific customer service scenarios. You are advised to use column-store tables for computing services (mainly involving association and aggregation operations) and row-store tables for point queries and massive **UPDATE** or **DELETE** executions.

For details about the optimization methods of each storage mode, see subsequent sections.

Optimizing Queries by Rewriting SQL Statements
----------------------------------------------

Besides the preceding methods that improve the performance of the execution plans generated by the SQL engine, database administrators can also enhance SQL statement performance by rewriting SQL statements while retaining the original service logic based on the execution mechanisms of databases and abundant practices.

This requires that database administrators are familiar with customer services and SQL statements. Some common SQL rewriting scenarios will be described in subsequent sections.
