:original_name: opengauss_01_0041.html

.. _opengauss_01_0041:

Scaling Up Storage Space
========================

Scenarios
---------

As more data is added, your GaussDB instance may start to run out of space. If the kernel system detects that the disk usage exceeds 85%, the instance is set to read-only and no data can be written to the instance. This section describes how to scale up the storage space of a DB instance. Services are not interrupted during storage scale-up.

Precautions
-----------

-  Within the maximum allowed range, usage cannot exceed 85% of the total storage after the scale-up.
-  If any node becomes faulty, contact the O&M engineers for troubleshooting before the scale-up.
-  The storage space must be a multiple of (Number of shards x 40 GB).

Constraints
-----------

-  The maximum allowed storage is 4 TB. There is no limit on the number of scale-ups.
-  The DB instance is in **Scaling up** state when its storage space is being scaled up and the backup services are not affected.
-  Reboot is not required when the instance is being scaled up.
-  You cannot reboot or delete an instance that is being scaled up.
-  Storage space can only be scaled up, not down.

Procedure
---------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. On the **Instances** page, locate the instance you want to scale up and click **More** > **Scale Storage Space** in the **Operation** column.

   Alternatively, click the instance name to go to the **Basic Information** page. In the **Storage/Backup Space** area, click **Scale**.

#. On the displayed page, specify the new storage space and click **Next**.

   When you scale up storage space, ensure that the usage of the new storage space is less than 85%. Once the storage usage of a DB instance reaches 85% or higher, the instance cannot process write operations and becomes read-only.

#. Confirm settings.

   -  If you need to modify your settings, click **Previous**.
   -  If your settings are correct, click **Submit**.

#. View the results.

   During the scale-up, the status of the instance on the **Instances** page is **Scaling up**. This process may take 3 to 5 minutes. Once the scaling is complete, click the instance name to go the **Basic Information** page and you can see the new storage space.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
