:original_name: opengauss_01_0034.html

.. _opengauss_01_0034:

Rebooting a DB Instance
=======================

Scenarios
---------

You can reboot a DB instance for the modifications to take effect.

.. important::

   -  You can reboot a DB instance only when its status is **Available**. Your database may be unavailable in some cases, for example, when some modifications are being made.
   -  Rebooting a DB instance will cause service interruptions. During this period, the DB instance status is **Rebooting**.
   -  An instance is not available when it is being rebooted. After the reboot completes, the cached memory will be automatically cleared. You are advised to reboot the instance during off-peak hours.
   -  To quickly reboot a DB instance, perform fewer operations on the DB instance.

Procedure
---------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. On the **Instances** page, locate the instance you want to reboot and choose **More** > **Reboot** in the **Operation** column.

   Alternatively, click the instance name to go to the **Basic Information** page. Click **Reboot** in the upper right corner of the page.

#. In the displayed dialog box, click **Yes**.

   The instance status becomes **Rebooting**.

#. Refresh the DB instance list and view the status of the DB instance. If its status is **Available**, it has been rebooted.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
