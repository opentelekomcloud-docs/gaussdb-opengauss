:original_name: opengauss_01_0065.html

.. _opengauss_01_0065:

Creating a Manual Backup for Instances
======================================

Scenarios
---------

GaussDB allows you to create instance-level manual backups for available instances. You can use these backups to restore data.

Precautions
-----------

-  Manual backups are user-initiated full backups of instances. They are retained until you delete them manually.

-  You can back up data of instances that are in the **Available** state.
-  A user can perform only one instance-level backup operation for a DB instance at a time.

Method 1
--------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. On the **Instances** page, locate the instance and choose **More** > **Create Backup** in the **Operation** column.

#. In the displayed dialog box, enter a backup name and description. Then, click **OK**. If you want to cancel the backup creation task, click **Cancel**.

   -  The backup name can contain 4 to 64 characters and must start with a letter. Only letters (case-sensitive), digits, hyphens (-), and underscores (_) are allowed.
   -  The description can contain up to 256 characters, but cannot contain carriage returns and the following special characters: >!<"&'=
   -  During the creation process, the instance status is **Backing up**. The time required for creating a manual backup depends on the data volume.

#. View and manage the created backup on the **Backups** page.

   Alternatively, click the instance name. On the **Backups** page, you can view and manage the manual backups.

Method 2
--------

#. Log in to the management console.

#. Click |image3| in the upper left corner and select a region and project.

#. Click |image4| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. On the **Instances** page, click the name of the target instance to go to the **Basic Information** page.

#. In the navigation pane on the left, choose **Backups**. On the displayed page, click **Create Backup**.

#. In the displayed dialog box, enter a backup name and description and click **OK**.

   -  The backup name can contain 4 to 64 characters and must start with a letter. Only letters (case-sensitive), digits, hyphens (-), and underscores (_) are allowed.
   -  The description can contain up to 256 characters, but cannot contain carriage returns and the following special characters: >!<"&'=
   -  During the creation process, the manual backup status is **Creating**. The time required for creating a manual backup depends on the data volume.

#. View and manage the created backup on the current page.

   Alternatively, go back to the instance list page, and click **Backups** to view and manage the backup.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
.. |image3| image:: /_static/images/en-us_image_0000002088517922.png
.. |image4| image:: /_static/images/en-us_image_0000002124197217.png
