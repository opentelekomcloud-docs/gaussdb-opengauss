:original_name: opengauss_opti_0045.html

.. _opengauss_opti_0045:

SQL Self-Diagnosis
==================

Performance issues may occur when you query data or run the **INSERT**, **DELETE**, **UPDATE**, or **CREATE TABLE AS** statement. In this case, you can query the **warning** column in the GS_WLM_SESSION_STATISTICS, GS_WLM_SESSION_HISTORY, and GS_WLM_SESSION_QUERY_INFO_ALL views to obtain reference for performance tuning.

Scenarios
---------

Currently, the following performance alarms will be reported:

-  Some column statistics are not collected.

An alarm will be reported if some column statistics are not collected. To update and optimize statistics, see :ref:`Updating Statistics <opengauss_opti_0035>` and :ref:`Tuning Statistics <opengauss_opti_0048>`.

An alarm will also be reported if column statistics are not collected for queries on OBS tables. The performance of the **ANALYZE** statement executed for these foreign tables is poor and seems inefficient for a simple query. Run this statement as needed.

Example alarms:

No statistics about a table are not collected.

.. code-block::

   Statistic Not Collect:
       schema_test.t1

The statistics about a single column are not collected.

.. code-block::

   Statistic Not Collect:
       schema_test.t2(c1,c2)

The statistics about multiple columns are not collected.

.. code-block::

   Statistic Not Collect:
       schema_test.t3((c1,c2))

The statistics about a single column and multiple columns are not collected.

.. code-block::

   Statistic Not Collect:
       schema_test.t4(c1,c2)    schema_test.t4((c1,c2))

-  SQL statements are not pushed down.

   The cause details are displayed in the alarms. For details about the optimization, see :ref:`Tuning Statement Pushdown <opengauss_opti_0046>`.

   -  If the pushdown failure is caused by functions, the function names are displayed in the alarm.
   -  If the pushdown failure is caused by statements, the specific statements are displayed. For example, the following statements do not support pushdown:

      -  Statements containing the **With Recursive**, **Distinct On**, or **Row** expression
      -  Statements whose return values are of record type

Example alarms:

.. code-block::

   SQL is not plan-shipping, reason : "With Recursive can not be shipped"
   SQL is not plan-shipping, reason : "Function now() can not be shipped"
   SQL is not plan-shipping, reason : "Function string_agg() can not be shipped"

-  In a hash join, the larger table is used as the inner table.

An alarm will be reported if the number of rows in the inner table reaches or exceeds 10 times of that in the outer table, more than 100,000 inner-table rows are processed on each DN in average, and the join statement has spilled to disk. You can view the **query_plan** column in the **GS_WLM_SESSION_HISTORY** view to check whether the hash join is used. For details about the optimization, see :ref:`Hint-based Tuning <opengauss_opti_0053>`.

Example alarms:

.. code-block::

   PlanNode[7] Large Table is INNER in HashJoin "Vector Hash Aggregate"

-  Nested loop is used in a large-table equi-join.

An alarm will be reported if nested loop is used in an equi-join where more than 100,000 larger-table rows are processed on each DN in average. You can view the **query_plan** column in the **GS_WLM_SESSION_HISTORY** view to check whether nested loop is used. For details about the optimization, see :ref:`Hint-based Tuning <opengauss_opti_0053>`.

Example alarms:

.. code-block::

   PlanNode[5] Large Table with Equal-Condition use Nestloop"Nested Loop"

-  A large table is broadcast.

An alarm will be reported if more than 100,000 rows are broadcast on each DN in average. For details about the optimization, see :ref:`Hint-based Tuning <opengauss_opti_0053>`.

Example alarms:

.. code-block::

   PlanNode[5] Large Table in Broadcast "Streaming(type: BROADCAST dop: 1/2)"

-  Data skew occurs.

An alarm will be reported if the number of rows processed on any DN exceeds 100,000, and the number of rows processed on a DN reaches or exceeds 10 times of that processed on another DN. For details about the optimization, see :ref:`Tuning Data Skew <opengauss_opti_0050>`.

Example alarms:

.. code-block::

   PlanNode[6] DataSkew:"Seq Scan", min_dn_tuples:0, max_dn_tuples:524288

-  Estimation is inaccurate.

An alarm will be reported if the maximum number or the estimated maximum number of rows processed on a DN exceeds 100,000, and the larger value reaches or exceeds 10 times of the smaller one. For details about the optimization, see :ref:`Hint-based Tuning <opengauss_opti_0053>`.

Example alarms:

.. code-block::

   PlanNode[5] Inaccurate Estimation-Rows: "Hash Join" A-Rows:0, E-Rows:52488

Restrictions
------------

#. An alarm contains up to 2048 characters. If the length of an alarm exceeds this value (for example, a large number of long table names and column names are displayed in the alarm when their statistics are not collected), a warning instead of an alarm will be reported.

   WARNING, "Planner issue report is truncated, the rest of planner issues will be skipped"

#. If a query statement contains the **Limit** operator, alarms of operators lower than **Limit** will not be reported.

#. For alarms about data skew and inaccurate estimation, only alarms on the lower-layer nodes in a plan tree will be reported. This is because the same alarms on the upper-level nodes may be triggered by problems on the lower-layer nodes. For example, if data skew occurs on the **Scan** node, data skew may also occur in operators (for example, **HashAgg**) at the upper layer.
