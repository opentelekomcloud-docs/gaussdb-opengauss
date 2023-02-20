:original_name: opengauss_01_0066.html

.. _opengauss_01_0066:

Restoring DB Instances to a Specific Point in Time
==================================================

Scenarios
---------

You can use an automated backup to restore a DB instance to a specified point in time.

Backups can be restored to a new DB instance.

Constraints
-----------

-  If you restore backup data to a new DB instance:

   -  The DB engine and major version are the same as those of the original DB instance and cannot be changed.
   -  You need to set a new administrator password.

**Procedure**
-------------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and a project.

#. Click **Service List**. Under **Database**, click **GaussDB**.

#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.

#. On the **Instance Management** page, click the target DB instance.

#. In the navigation pane on the left, choose **Backups & Restorations**. On the displayed page, click **Restore to Point in Time**.

#. Select a restoration date and time range, enter a time point within the selected time range, and click **OK**.

   Restoring data to a new DB instance

   The **Create New Instance** page is displayed.

   -  The DB engine and version of the new DB instance are the same as those of the original DB instance and cannot be changed.
   -  Storage space of the new DB instance is the same as that of the original DB instance by default and cannot be less than that of the original DB instance. The administrator password needs to be reset.
   -  The instance class of the new DB instance is the same as that of the original DB instance by default and must be greater than or equal to that of the original DB instance.
   -  The number of CNs and shards of the new DB instance must be the same as that of the original DB instance during the backup.

   Select an existing DB instance and click **OK**.

#. View the restoration result.

   Restoring data to a new DB instance

   A new DB instance is created using the backup data. The status of the DB instance changes from **Creating** to **Available**.

   The new DB instance is independent from the original one.

   On the **Instance Management** page, the status of the DB instance changes from **Restoring** to **Available**.

   A full backup is triggered after the new DB instance is created.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
