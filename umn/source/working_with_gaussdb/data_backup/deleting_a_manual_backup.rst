:original_name: opengauss_01_0069.html

.. _opengauss_01_0069:

Deleting a Manual Backup
========================

Scenarios
---------

You can delete manual backups for instances and tables to release storage space.

.. important::

   -  Deleted manual backups cannot be recovered. Exercise caution when performing this operation.
   -  Automated backups cannot be manually deleted.
   -  Backups that are being restored cannot be deleted.
   -  To delete a backup, you must log in to the account that the backup belongs to.

Procedure
---------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. In the navigation pane on the left, choose **Backups**. On the displayed page, locate the manual backup to be deleted and click **Delete** in the **Operation** column.

   Alternatively, on the **Instances** page, click the instance name to go to the **Basic Information** page. On the **Backups** page, locate the backup you want to delete and click **Delete** in the **Operation** column.

#. Click **Yes**.

   After the backup is deleted, it will not be displayed on the **Backups** page.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
