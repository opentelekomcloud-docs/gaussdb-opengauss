:original_name: opengauss_opti_0056.html

.. _opengauss_opti_0056:

Join Operation Hints
====================

Description
-----------

These hints specify the join method, which can be **Nested Loop**, **Hash Join**, or **Merge Join**.

Syntax
------

.. code-block::

   [no] nestloop|hashjoin|mergejoin(table_list)

.. _en-us_topic_0000002088517390__section16139mcpsimp:

Parameter Description
---------------------

-  **no** specifies that the specified hint will not be used for a join.
-  *table_list* specifies the tables to be joined. The values are the same as those of :ref:`join_table_list <opengauss_opti_0055>` but contain no parentheses.

Example:

**no nestloop(t1 t2 t3)**: **nestloop** is not used for joining **t1**, **t2**, and **t3**. The three tables may be joined in either of the two ways: Join **t2** and **t3**, and then **t1**; Join **t1** and **t2**, and then **t3**. This hint takes effect only for the last join. If necessary, you can hint other joins. For example, you can add **no nestloop(t2 t3)** to join **t2** and **t3** first and to forbid the use of **nestloop**.

Example
-------

Hint the query plan in :ref:`Example <en-us_topic_0000002124277245__section16089mcpsimp>` as follows:

.. code-block::

   explain
   select /*+ nestloop(store_sales store_returns item) */ i_product_name product_name ...

**nestloop** is used for the last join between **store_sales**, **store_returns**, and **item**. The optimized plan is as follows:

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002124197281.png
