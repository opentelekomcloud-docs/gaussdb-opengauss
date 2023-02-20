:original_name: opengauss_01_0034.html

.. _opengauss_01_0034:

Rebooting a DB Instance
=======================

Scenarios
---------

This section describes how to reboot a DB instance for the modifications to take effect.

.. important::

   -  You can reboot a DB instance only when its status is **Available**. Your database may be unavailable in some cases such as some modifications are being made.
   -  Rebooting DB instances will reboot database services. Rebooting a DB instance will cause service interruption. During this period, the DB instance status is **Rebooting**.
   -  This DB instance will not be available when it is being rebooted. After the reboot completes, the cached memory will be automatically cleared. The DB instance needs to be warmed up to prevent congestion during peak hours.
   -  When a DB instance is abnormal, rebooting the DB instance cannot make it normal. Exercise caution when performing this operation.

Procedure
---------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and a project.

#. Click **Service List**. Under **Database**, click **GaussDB**.

#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.

#. On the **Instance Management** page, locate the target DB instance and choose **More** > **Reboot** in the **Operation** column to reboot the DB instance.

   Alternatively, click the target instance. On the displayed page, click **Reboot** in the upper right corner of the page.

#. In the displayed dialog box, click **Yes**.

#. Refresh the DB instance list and view the status of the DB instance. If its status is **Available**, it is rebooted successfully.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
