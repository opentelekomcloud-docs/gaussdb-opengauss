:original_name: opengauss_01_0069.html

.. _opengauss_01_0069:

Deleting a Manual Backup
========================

Scenarios
---------

You can delete manual backups to release storage space.

.. important::

   Deleted manual backups cannot be recovered. Exercise caution when performing this operation.

   -  If a backup set is being restored, it cannot be deleted.

**Procedure**
-------------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and a project.

#. Click **Service List**. Under **Database**, click **GaussDB**.

#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.

#. In the navigation pane on the left, choose **Backup Management**. On the displayed page, locate the target manual backup to be deleted and click **Delete** in the **Operation** column.

   The following backups cannot be deleted:

   -  Automated backups when the backup police is enabled
   -  Backups that are being restored

#. Click **Yes**.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
