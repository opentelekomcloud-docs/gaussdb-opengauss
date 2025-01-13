:original_name: opengauss_opti_0064.html

.. _opengauss_opti_0064:

Checking the Implicit Conversion Performance
============================================

In some scenarios, implicit data type conversion may cause performance problems. For example:

.. code-block::

   SET enable_fast_query_shipping = off;
   CREATE TABLE t1(c1 VARCHAR, c2 VARCHAR);
   CREATE INDEX on t1(c1);
   EXPLAIN verbose SELECT * FROM t1 WHERE c1 = 10;

The execution plan of the preceding query is as follows:

|image1|

The data type of **c1** is **varchar**. When the filter criterion is **c1 = 10**, the optimizer implicitly converts the data type of **c1** to **bigint** by default. As a result, the following consequences occur:

-  DNs cannot be tailored. The plan is delivered to all DNs for execution.
-  Index Scan cannot be used to scan data in the plan.

These may cause performance problems.

After knowing the causes, you can rewrite the SQL statements. In the preceding scenario, you only need to convert the constant display in the filter criteria to the **varchar** type. The result is as follows:

.. code-block::

   EXPLAIN verbose SELECT * FROM t1 WHERE c1 = 10::varchar;

|image2|

To identify the performance impact of implicit type conversion in advance, you can use the GUC parameter **check_implicit_conversions**. After this parameter is enabled, the system checks the index columns that are implicitly converted in the query in the path generation phase. If no candidate index scan path is generated for the index columns, an error message is displayed. For example:

.. code-block::

   SET check_implicit_conversions = on;
   SELECT * FROM t1 WHERE c1 = 10;
   ERROR:  There is no optional index path for index column: "t1"."c1".
   Please check for potential performance problem.

.. note::

   -  The **check_implicit_conversions** parameter is used only to check for potential performance problems caused by implicit type conversion. In the formal production environment, set this parameter to **off** (default value).

   -  When enabling **check_implicit_conversions**, you must disable **enable_fast_query_shipping**. Otherwise, you cannot view the result of restoring the implicit type conversion.

   -  A candidate path of a table may include multiple possible data scan modes, such as seq scan and index scan. A table scan mode used in the final execution plan is determined by the cost of the execution plan. Therefore, even if a candidate path for index scan is generated, other scan modes may also be used in the final execution plan.

.. |image1| image:: /_static/images/en-us_image_0000002088677786.png
.. |image2| image:: /_static/images/en-us_image_0000002124277521.png
