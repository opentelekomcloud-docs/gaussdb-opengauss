:original_name: opengauss_01_0041.html

.. _opengauss_01_0041:

Scaling Up Storage Space
========================

Scenarios
---------

The storage space may be insufficient as your service volumes grow. If the kernel detects that the disk usage exceeds 85%, the DB instance is set to read-only and no data can be written to the DB instance. This section describes how to scale up the storage space of a DB instance.

Precautions
-----------

-  Within the maximum allowed range, disk usage cannot exceed 85% of the total storage space after disk expansion.
-  If any node becomes faulty, contact the O&M engineers for troubleshooting.
-  The storage space must be a multiple of the number of shards multiplied by 40 GB.
-  A shard disk has a maximum capacity of 16TB, and 16TB capacity is added each time a shard is added.

**Procedure**
-------------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and a project.

#. Click **Service List**. Under **Database**, click **GaussDB**.

#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.

#. On the **Instance Management** page, locate the target DB instance and click **More** > **Scale Storage Space** in the **Operation** column.

   Alternatively, click the target DB instance to go to the **Basic Information** page. In the **Storage/Backup Space** area, click **Scale**.

#. On the displayed page, specify the new storage space and click **Next**.

#. Confirm specifications.

   -  If you need to modify your settings, click **Previous**.

#. View the scale-up result.

   During the scale-up, the status of the DB instance on the **Instance Management** page will be **Scaling up**. Click the DB instance and view the utilization on the displayed **Basic Information** page to verify that the scale-up is successful. This process may take 3 to 5 minutes.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
