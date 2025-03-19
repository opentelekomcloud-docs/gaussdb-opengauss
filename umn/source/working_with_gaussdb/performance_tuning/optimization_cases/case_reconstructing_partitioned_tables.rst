:original_name: opengauss_opti_0072.html

.. _opengauss_opti_0072:

Case: Reconstructing Partitioned Tables
=======================================

Symptom
-------

In the following simple SQL statements, the performance bottlenecks exist in the scan operation on the **dwcjk** table.

|image1|

Optimization Analysis
---------------------

Obviously, there are date features in the **cjrq** field of table data in the service layer, and this meet the features of a partitioned table. Replan the table definition of the **dwcjk** table: Use the **cjrq** field as a partition key, and day as an interval unit to define the partitioned table **dwcjk_part**. The modified result is as follows, and the performance is nearly doubled.

|image2|

.. |image1| image:: /_static/images/en-us_image_0000002124277605.jpg
.. |image2| image:: /_static/images/en-us_image_0000002124197309.jpg
