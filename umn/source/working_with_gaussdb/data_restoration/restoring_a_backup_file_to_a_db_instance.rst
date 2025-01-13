:original_name: opengauss_01_0068.html

.. _opengauss_01_0068:

Restoring a Backup File to a DB Instance
========================================

Scenarios
---------

You can use an instance-level automated or manual backup to restore data to the point in time when the backup was created. The restoration is at the DB instance level.

Data can be restored to a new DB instance.

Constraints
-----------

-  Restoration will fail if the instance is in the **Abnormal**, or **Storage full** state.
-  GaussDB currently only supports restoration between DB instances running the same major version. For example, backup data can only be restored from version 1.4.\ *x* to version 1.4.\ *y*.

Procedure
---------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. On the **Backups** page, locate the backup to be restored and click **Restore** in the **Operation** column.

   Alternatively, on the **Instances** page, click the instance name to go to the **Basic Information** page. In the navigation pane on the left, choose **Backups**. Click **Instance Backup** under **Full Backups**. On the displayed page, select the backup to be restored and click **Restore** in the **Operation** column.

#. Click **OK**.

   .. note::

      -  Parallel restoration cannot be enabled if the DB engine version is earlier than 1.4.
      -  In addition to full backups and incremental backups, the system also backs up incremental log files to ensure data consistency. It takes some time to back up and upload incremental log files (The time depends on the network and OBS traffic control). If you have strict requirements on data consistency after restoration, you can restore data to a specified point in time.

   -  Restoring data to a new DB instance:

      -  The original and new DB instances must have the same major version. For example, backup data can only be restored from version 1.4.\ *x* to version 1.4.\ *y*.
      -  The storage space of the new instance is the same as that of the original DB instance by default and the new instance must be at least as large as the original DB instance.
      -  The administrator password needs to be reset.
      -  By default, the instance specifications of the new DB instance are the same as those of the original DB instance. To change the instance specifications, ensure that the instance specifications of the new DB instance are at least those of the original DB instance.
      -  The new DB instance has the same node configurations as the original DB instance.

      Configure the basic information about the new instance and click **Next**.

#. View the restoration results.

   -  Restoring data to a new DB instance

      A new DB instance is created using the backup data. The status of the DB instance changes from **Creating** to **Available**.

      The new DB instance is independent from the original one.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
