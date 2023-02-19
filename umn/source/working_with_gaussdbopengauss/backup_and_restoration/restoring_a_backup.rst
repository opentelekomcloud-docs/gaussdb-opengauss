:original_name: opengauss_01_0068.html

.. _opengauss_01_0068:

Restoring a Backup
==================

Scenarios
---------

You can use an automated or manual backup to restore data to the status when the backup was created. The restoration is at the DB instance level.

Constraints
-----------

-  Cross-version restoration is not supported.

**Procedure**
-------------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and a project.

#. Click **Service List**. Under **Database**, click **GaussDB**.

#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.

#. On the **Backup Management** page, select the backup to be restored and click **Restore** in the **Operation** column.

   Alternatively, click the target DB instance on the **Instance Management** page. On the displayed page, choose **Backups & Restorations**. On the displayed page, select the target backup to be restored and click **Restore** in the **Operation** column.

#. Select a restoration method and click **OK**.

   The **Create New Instance** page is displayed.

   -  The DB engine and version of the new DB instance are the same as those of the original DB instance.
   -  Storage space of the new DB instance is the same as the backup space by default and must be greater than or equal to the backup space. The administrator password needs to be reset.
   -  The instance class of the new DB instance is the same as that of the original DB instance by default and must be greater than or equal to that of the original DB instance.
   -  The number of CNs and shards of the new DB instance must be the same as that of the original DB instance during the backup.

#. View the restoration result.

   Restoring data to a new DB instance

   A new DB instance is created using the backup data. The status of the DB instance changes from **Creating** to **Available**.

   The new DB instance is independent from the original one.

   After the new DB instance is created, the system will perform a full backup.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
