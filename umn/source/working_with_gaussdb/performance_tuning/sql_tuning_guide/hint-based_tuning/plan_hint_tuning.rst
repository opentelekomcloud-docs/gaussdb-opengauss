:original_name: opengauss_opti_0054.html

.. _opengauss_opti_0054:

Plan Hint Tuning
================

In plan hints, you can specify the following information to tune an execution plan, improving query performance.

-  Join order
-  Join, stream, and scan operations
-  Number of rows in a result
-  Redistribution skew information

Description
-----------

The hint syntax must follow immediately after a **SELECT** keyword and is written in the following format:

.. code-block::

   /*+ <plan hint>*/

You can specify multiple hints for a query plan and separate them by spaces. A hint specified for a query plan does not apply to its subquery plans. To specify a hint for a subquery, add the hint following the **SELECT** of this subquery.

Example:

.. code-block::

   select /*+ <plan_hint1> <plan_hint2> */ * from t1, (select /*+ <plan_hint3> */ from t2) where 1=1;

In the preceding command, <*plan_hint1*> and <*plan_hint2*> are the hints of a query, and <*plan_hint3*> is the hint of its subquery.

.. important::

   If a hint is specified in the **CREATE VIEW** statement, the hint will be applied each time this view is used.

.. note::

   If the random plan function is enabled (**plan_mode_seed** is set to a value other than 0), the specified hint will not be used.

Supported Hints
---------------

Currently, the following hints are supported:

-  Join order hints (**leading**)
-  Join operation hints, excluding the **semi join**, **anti join**, and **unique plan** hints
-  Rows hints
-  Stream operation hints
-  Scan operation hints, supporting only **tablescan**, **indexscan**, and **indexonlyscan**
-  Sublink name hints
-  Skew hints, supporting only the skew in the redistribution involving Join or HashAgg

Precautions
-----------

-  Hints do not support **Agg**, **Sort**, **Setop**, or **Subplan**.
-  Hints do not support SMP or Node Groups.

.. _en-us_topic_0000002124277245__section16089mcpsimp:

Example
-------

The following is the original plan and is used for comparing with the optimized ones:

.. code-block::

   explain
   select i_product_name product_name
   ,i_item_sk item_sk
   ,s_store_name store_name
   ,s_zip store_zip
   ,ad2.ca_street_number c_street_number
   ,ad2.ca_street_name c_street_name
   ,ad2.ca_city c_city
   ,ad2.ca_zip c_zip
   ,count(*) cnt
   ,sum(ss_wholesale_cost) s1
   ,sum(ss_list_price) s2
   ,sum(ss_coupon_amt) s3
   FROM   store_sales
   ,store_returns
   ,store
   ,customer
   ,promotion
   ,customer_address ad2
   ,item
   WHERE  ss_store_sk = s_store_sk AND
   ss_customer_sk = c_customer_sk AND
   ss_item_sk = i_item_sk and
   ss_item_sk = sr_item_sk and
   ss_ticket_number = sr_ticket_number and
   c_current_addr_sk = ad2.ca_address_sk and
   ss_promo_sk = p_promo_sk and
   i_color in ('maroon','burnished','dim','steel','navajo','chocolate') and
   i_current_price between 35 and 35 + 10 and
   i_current_price between 35 + 1 and 35 + 15
   group by i_product_name
   ,i_item_sk
   ,s_store_name
   ,s_zip
   ,ad2.ca_street_number
   ,ad2.ca_street_name
   ,ad2.ca_city
   ,ad2.ca_zip
   ;

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002088678006.png
