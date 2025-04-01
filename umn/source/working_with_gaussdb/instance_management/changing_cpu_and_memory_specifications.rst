:original_name: opengauss_01_scale.html

.. _opengauss_01_scale:

Changing CPU and Memory Specifications
======================================

Scenarios
---------

You can change the CPU and memory specifications of a GaussDB instance if needed.

Precautions
-----------

-  By default, the specifications cannot be reduced. If you need to reduce the specifications, contact customer service.
-  Before you change the instance specifications, ensure that the instance is available. If the instance or node is abnormal, or the storage space is full, you cannot perform this operation.
-  During the specification change for an HA (1 primary + 2 standby) instance, a primary/standby failover is triggered. During the failover, services are interrupted for about 1 minute.
-  After you change vCPUs and memory, the DB instances will be rebooted and service will be interrupted. You are advised to perform this operation during off-peak hours.

Procedure
---------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. On the **Instances** page, locate the instance and choose **More** > **Change Instance Specifications** in the **Operation** column.

   Alternatively, click the instance name to go to the **Basic Information** page. In the **DB Information** area, click **Change** in the **Instance Specifications** field.

#. On the displayed page, specify the new instance specifications and click **Next**.

#. Confirm the specifications and click **Submit**.

#. View the new instance specifications.

   After the task is submitted, click **Go to Instance List**. On the **Instances** page, the DB instance status is **Changing instance specifications**. After a few minutes, view the new instance specifications on the **Basic Information** page.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
