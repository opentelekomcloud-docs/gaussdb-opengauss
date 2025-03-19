:original_name: opengauss_opti_0071.html

.. _opengauss_opti_0071:

Case: Adjusting Distribution Keys
=================================

Symptom
-------

During a site test, the information is displayed after **EXPLAIN ANALYZE** is run:

|image1|

According to the execution information, Hash Join becomes the performance bottleneck of the whole plan. Based on the execution time of hash join **[2657.406,93339.924]** (for details about the value, see :ref:`Description <opengauss_opti_0033>`), it can be seen that severe skew occurs on different DNs during the Hash Join operation.

In the memory information (as shown in the following figure), it can be seen that the data skew occurs in the memory usage of each node.

|image2|

Optimization Analysis
---------------------

The preceding two features indicate that this SQL statement has extremely serious computing unbalance. The further lower-layer analysis on the hash join operator shows that serious computing skew **[38.885,2940.983]** occurs in **Seq Scan on s_riskrate_setting**. Based on the description of the Scan, it is suspected that the performance problems of this plan may lie in data skew occurred in the **s_riskrate_setting** table. Later, it is proved that serious data skew occurred in the **s_riskrate_setting** table. After performance optimization, the execution time is reduced from 94s to 50s.

.. |image1| image:: /_static/images/en-us_image_0000002088518006.jpg
.. |image2| image:: /_static/images/en-us_image_0000002124277593.png
