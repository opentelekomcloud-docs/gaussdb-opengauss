:original_name: opengauss_opti_0063.html

.. _opengauss_opti_0063:

Plan Hint Cases
===============

This section takes the statements in TPC-DS (Q24) as an example to describe how to optimize an execution plan by using hints in 1000X+24DN environments. The following is an example:

.. code-block::

   select avg(netpaid) from
   (select c_last_name
   ,c_first_name
   ,s_store_name
   ,ca_state
   ,s_state
   ,i_color
   ,i_current_price
   ,i_manager_id
   ,i_units
   ,i_size
   ,sum(ss_sales_price) netpaid
   from store_sales
   ,store_returns
   ,store
   ,item
   ,customer
   ,customer_address
   where ss_ticket_number = sr_ticket_number
   and ss_item_sk = sr_item_sk
   and ss_customer_sk = c_customer_sk
   and ss_item_sk = i_item_sk
   and ss_store_sk = s_store_sk
   and c_birth_country = upper(ca_country)
   and s_zip = ca_zip
   and s_market_id=7
   group by c_last_name
   ,c_first_name
   ,s_store_name
   ,ca_state
   ,s_state
   ,i_color
   ,i_current_price
   ,i_manager_id
   ,i_units
   ,i_size);

#. The original plan of this statement is as follows and the statement execution takes 110s.

|image1|

In this plan, the performance of the layer-10 **broadcast** is poor because the estimation result generated at layer 11 is 2140 rows, much less than the actual number of rows. The inaccurate estimation is mainly caused by the underestimated number of rows in layer-13 hash join. In this layer, **store_sales** and **store_returns** are joined (based on the **ss_ticket_number** and **ss_item_sk** columns in **store_sales** and the **sr_ticket_number** and **sr_item_sk** columns in **store_returns**) but the multi-column correlation is not considered.

2. After the **rows** hint is used for optimization, the plan is as follows and the statement execution takes 318s:

.. code-block::

   select avg(netpaid) from
   (select /*+rows(store_sales store_returns * 11270)*/ c_last_name ...

|image2|

The execution takes a longer time because layer-9 **redistribute** is slow. Considering that data skew does not occur at layer-9 **redistribute**, the slow redistribution is caused by the slow layer-8 **hash join** due to data skew at layer-18 **redistribute**.

3. Data skew occurs because **customer_address** has a few different values in its two join keys. Therefore, plan **customer_address** as the last one to be joined. After the hint is used for optimization, the plan is as follows and the statement execution takes 116s.

.. code-block::

   select avg(netpaid) from
   (select /*+rows(store_sales store_returns *11270)
   leading((store_sales store_returns store item customer) customer_address)*/
   c_last_name ...

|image3|

Most of the time is spent on layer-6 **redistribute**. The plan needs to be further optimized.

4. Most of the time is spent on layer-6 **redistribute** because of data skew. To avoid the data skew, plan the **item** table as the last one to be joined because the number of rows is not reduced after **item** is joined. After the hint is used for optimization, the plan is as follows and the statement execution takes 120s.

.. code-block::

   select avg(netpaid) from
   (select /*+rows(store_sales store_returns *11270)
   leading((customer_address (store_sales store_returns store customer) item))
   c_last_name ...

|image4|

Data skew occurs after the join of **item** and **customer_address** because **item** is broadcast at layer-22. As a result, **redistribute** is still slow.

5. Add a hint to disable **broadcast** for **item** or add a **redistribute** hint for the join result of **item** and **customer_address**. After the hint is used for optimization, the plan is as follows and the statement execution takes 105s:

.. code-block::

   select avg(netpaid) from
   (select /*+rows(store_sales store_returns *11270)
   leading((customer_address (store_sales store_returns store customer) item))
   no broadcast(item)*/
   c_last_name ...

|image5|

6. The last layer uses single-layer **Agg** and the number of rows is greatly reduced. Set **best_agg_plan** to **3** and change the single-layer **Agg** to a double-layer **Agg**. The plan is as follows and the statement execution takes 94s. The optimization ends.

|image6|

If the query performance deteriorates due to statistics changes, you can use hints to optimize the query plan. Take TPCH-Q17 as an example. The query performance deteriorates after the value of **default_statistics_target** is changed from the default one to **-2** for statistics collection.

1. If **default_statistics_target** is set to the default value **100**, the plan is as follows:

|image7|

2. If **default_statistics_target** is set to **-2**, the plan is as follows:

|image8|

3. After the analysis, the cause is that the stream type is changed from **Broadcast** to **Redistribute** during the join of the **lineitem** and **part** tables. You can use a hint to change the stream type back to **BroadCast**. For example:

|image9|

.. |image1| image:: /_static/images/en-us_image_0000002088677810.png
.. |image2| image:: /_static/images/en-us_image_0000002088517970.png
.. |image3| image:: /_static/images/en-us_image_0000002124277549.png
.. |image4| image:: /_static/images/en-us_image_0000002124277565.png
.. |image5| image:: /_static/images/en-us_image_0000002124197265.png
.. |image6| image:: /_static/images/en-us_image_0000002124277545.png
.. |image7| image:: /_static/images/en-us_image_0000002088517962.png
.. |image8| image:: /_static/images/en-us_image_0000002088517954.png
.. |image9| image:: /_static/images/en-us_image_0000002088677818.png
