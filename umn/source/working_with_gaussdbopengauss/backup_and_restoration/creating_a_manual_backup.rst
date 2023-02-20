:original_name: opengauss_01_0065.html

.. _opengauss_01_0065:

Creating a Manual Backup
========================

Scenarios
---------

GaussDB(openGauss) allows you to create manual backups of a running DB instance. You can use these backups to restore data.

.. note::

   When a DB instance is deleted, its automated backups are also deleted, but its manual backups are not deleted.

**Precautions**
---------------

-  You can back up data of DB instances that are in the **Available** state.
-  On a DB instance, a user can perform only one backup operation at a time.

Method 1
--------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and a project.

#. Click **Service List**. Under **Database**, click **GaussDB**.

#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.

#. On the **Instance Management** page, locate the target DB instance and choose **Create Backup** in the **Operation** column.

#. In the displayed dialog box, enter a backup name and description. Then, click **OK**. If you want to cancel the backup creation task, click **Cancel**.

   -  The backup name must consist of 4 to 64 characters and start with a letter. It can contain only uppercase letters, lowercase letters, digits, hyphens (-), and underscores (_).
   -  The description consists of a maximum of 256 characters and cannot contain the carriage return character or the following special characters: >!<"&'=
   -  During the creation process, the backup status is **Creating**. The time required for creating a manual backup depends on the data volume.

#. After a manual backup has been created, you can view and manage it on the **Backup Management** page.

   Alternatively, click the target instance. On the **Backups & Restorations** page, you can view and manage the manual backups.

Method 2
--------

#. Log in to the management console.

#. Click |image2| in the upper left corner and select a region and a project.

#. Click **Service List**. Under **Database**, click **GaussDB**.

#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.

#. On the **Instance Management** page, click the target DB instance.

#. On the **Backups & Restorations** page, click **Create Backup**. In the displayed dialog box, enter a backup name and a description, and click **OK**. If you want to cancel the backup creation task, click **Cancel**.

   -  The backup name must consist of 4 to 64 characters and start with a letter. It can contain only uppercase letters, lowercase letters, digits, hyphens (-), and underscores (_).
   -  The description consists of a maximum of 256 characters and cannot contain the carriage return character or the following special characters: >!<"&'=
   -  During the creation process, the instance status is **Backing up**. The time required for creating a manual backup depends on the data volume.

#. After a manual backup has been created, you can view and manage it on the **Backup Management** page.

   Alternatively, click the target instance. On the **Backups & Restorations** page, you can view and manage the manual backups.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
.. |image2| image:: /_static/images/en-us_image_0000001072358973.png
