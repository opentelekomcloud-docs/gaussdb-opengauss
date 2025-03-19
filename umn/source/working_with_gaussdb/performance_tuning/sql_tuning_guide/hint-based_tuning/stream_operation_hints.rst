:original_name: opengauss_opti_0058.html

.. _opengauss_opti_0058:

Stream Operation Hints
======================

Description
-----------

These hints specify a stream operation, which can be **broadcast** or **redistribute**.

Syntax
------

.. code-block::

   [no] broadcast|redistribute(table_list)

Parameter Description
---------------------

-  **no** specifies that the specified hint will not be used for a stream operation.
-  *table_list* indicates tables to be joined for a stream operation. For details, see :ref:`Parameter description <en-us_topic_0000002088517390__section16139mcpsimp>`.

Example
-------

Hint the query plan in :ref:`Example <en-us_topic_0000002124277245__section16089mcpsimp>` as follows:

.. code-block::

   explain
   select /*+ no redistribute(store_sales store_returns item store) leading(((store_sales store_returns item store) customer)) */ i_product_name product_name ...

In the original plan, the join result of **store_sales**, **store_returns**, **item**, and **store** is redistributed before it is joined with **customer**. After the hinting, the redistribution is disabled and the join order is retained. The optimized plan is as follows:

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002088678154.png
