:original_name: opengauss_opti_0055.html

.. _opengauss_opti_0055:

Join Order Hints
================

Description
-----------

Theses hints specify the join order and outer/inner tables.

Syntax
------

-  Specify only the join order.

.. code-block::

   leading(join_table_list)

-  Specify both the join order and outer/inner tables. The outer/inner tables are specified by the outermost parentheses.

.. code-block::

   leading((join_table_list))

Parameter Description
---------------------

*join_table_list* specifies the tables to be joined. The values can be table names or table aliases. If a subquery is pulled up, the value can also be the subquery alias. Separate the values with spaces. You can add parentheses to specify the join priorities of tables.

.. important::

   A table name or alias can only be a string without a schema name.

.. note::

   An alias (if any) is used to represent a table.

To prevent semantic errors, tables in the list must meet the following requirements:

-  The tables must exist in the query or its subquery to be pulled up.
-  The table names must be unique in the query or subquery to be pulled up. If they are not, their aliases must be unique.
-  A table appears only once in the list.
-  An alias (if any) is used to represent a table.

Example:

**leading(t1 t2 t3 t4 t5)**: **t1**, **t2**, **t3**, **t4**, and **t5** are joined. The join order and outer/inner tables are not specified.

**leading((t1 t2 t3 t4 t5))**: **t1**, **t2**, **t3**, **t4**, and **t5** are joined in sequence. The table on the right is used as the inner table in each join.

**leading(t1 (t2 t3 t4) t5)**: First, **t2**, **t3**, and **t4** are joined and the outer/inner tables are not specified. Then, the result is joined with **t1** and **t5**, and the outer/inner tables are not specified.

**leading((t1 (t2 t3 t4) t5))**: First, **t2**, **t3**, and **t4** are joined and the outer/inner tables are not specified. Then, the result is joined with **t1**, and **(t2 t3 t4)** is used as the inner table. Finally, the result is joined with **t5**, and **t5** is used as the inner table.

**leading((t1 (t2 t3) t4 t5)) leading((t3 t2))**: First, **t2** and **t3** are joined and **t2** is used as the inner table. Then, the result is joined with **t1**, and **(t2 t3)** is used as the inner table. Finally, the result is joined with **t4** and then **t5**, and the table on the right in each join is used as the inner table.

Example
-------

Hint the query plan in :ref:`Example <en-us_topic_0000002124277245__section16089mcpsimp>` as follows:

.. code-block::

   explain
   select /*+ leading((((((store_sales store) promotion) item) customer) ad2) store_returns) leading((store store_sales))*/ i_product_name product_name ...

First, **store_sales** and **store** are joined and **store_sales** is the inner table. Then, the result is joined with **promotion**, **item**, **customer**, **ad2**, and **store_returns** in sequence. The optimized plan is as follows:

|image1|

For details about the warning at the top of the plan, see :ref:`Hint Errors, Conflicts, and Other Warnings <opengauss_opti_0062>`.

.. |image1| image:: /_static/images/en-us_image_0000002088678138.png
