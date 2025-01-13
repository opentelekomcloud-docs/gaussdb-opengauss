:original_name: opengauss_opti_0059.html

.. _opengauss_opti_0059:

Scan Operation Hints
====================

Description
-----------

These hints specify a scan operation, which can be **tablescan**, **indexscan**, or **indexonlyscan**.

Syntax
------

.. code-block::

   [no] tablescan|indexscan|indexonlyscan(table [index])

Parameter Description
---------------------

-  **no** specifies that the specified hint will not be used for a scan operation.
-  *table* specifies the table to be scanned. You can specify only one table and use a table alias (if any) instead of a table name.
-  *index* specifies the index for **indexscan** or **indexonlyscan**. You can specify only one index.

**indexscan** and **indexonlyscan** hints can be used only when the specified index belongs to the table.

Scan operation hints can be used for row-store tables, column-store tables, OBS tables, and subquery tables.

Example
-------

To specify an index-based hint for a scan, create an index named **i** on the **i_item_sk** column of the **item** table.

.. code-block::

   create index i on item(i_item_sk);

Hint the query plan in :ref:`Example <en-us_topic_0000002124277245__section16089mcpsimp>` as follows:

.. code-block::

   explain
   select /*+ indexscan(item i) */ i_product_name product_name ...

**item** is scanned based on an index. The optimized plan is as follows:

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002088518002.png
