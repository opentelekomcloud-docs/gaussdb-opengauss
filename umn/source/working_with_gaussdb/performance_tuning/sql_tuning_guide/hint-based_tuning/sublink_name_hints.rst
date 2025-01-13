:original_name: opengauss_opti_0060.html

.. _opengauss_opti_0060:

Sublink Name Hints
==================

Description
-----------

These hints specify the name of a sublink block.

Syntax
------

.. code-block::

   blockname (table)

Parameter Description
---------------------

-  *table* specifies the name you have specified for a sublink block.

.. note::

   -  The **blockname** hint is used by an outer query only when a sublink is pulled up. Currently, only the **Agg** equivalent join, **IN**, and **EXISTS** sublinks can be pulled up. This hint is usually used together with the hints described in the previous sections.

   -  The subquery after the **FROM** keyword is hinted by using the subquery alias. In this case, **blockname** becomes invalid.

   -  If a sublink contains multiple tables, the tables will be joined with the outer-query tables in a random sequence after the sublink is pulled up. In this case, **blockname** also becomes invalid.

Example
-------

.. code-block::

   explain select /*+nestloop(store_sales tt) */ * from store_sales where ss_item_sk in (select /*+blockname(tt)*/ i_item_sk from item group by 1);

**tt** indicates the sublink block name. After being pulled up, the sublink is joined with the outer-query table **store_sales** by using **nestloop**. The optimized plan is as follows:

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002124197233.png
