:original_name: opengauss_01_0066.html

.. _opengauss_01_0066:

Restoring a DB Instance to a Specific Point in Time
===================================================

Scenarios
---------

You can use an instance-level automated backup to restore a GaussDB instance to a specified point in time.

You can restore data to a new instance.

Precautions
-----------

-  Only instances of version 2.1 or later can be restored to any point in time.
-  If nodes are being added, versions are being upgraded, or data is being restored to an existing instance, the instance cannot be restored a specific point in time.
-  If a DB instance is faulty or a CN is removed, archive logs cannot be generated and the instance cannot be restored to a specific point in time.
-  If you restore backup data to a new DB instance:

   -  The DB engine and major version are the same as those of the original DB instance and cannot be changed.
   -  The administrator password needs to be reset.

-  If you restore backup data to the original DB instance, data on the original instance will be overwritten and the original DB instance will be unavailable during the restoration. Additionally, log archiving stops. After the restoration is complete, the **Confirm Data Integrity** button is displayed. Before clicking **Confirm Data Integrity**, you can restore data for multiple times. Once data integrity has been confirmed, any logs archived after the point in time data was restored from will be lost, but normal log archiving will be restored.
-  When a DB instance is deleted, all archive logs are deleted by default and cannot be retained. Data cannot be restored to a specific point in time after the DB instance is rebuilt.

**Procedure**
-------------

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and project.
#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.
#. On the **Instances** page, click the name of the target instance to go to the **Basic Information** page.
#. In the navigation pane on the left, choose **Backups**. On the displayed page, click **Restore to Point in Time**.
#. Click **OK**.

   .. note::

      -  Parallel restoration cannot be enabled if the DB engine version is earlier than 1.4.

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
