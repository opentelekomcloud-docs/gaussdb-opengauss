:original_name: opengauss_01_0035.html

.. _opengauss_01_0035:

Deleting a DB Instance
======================

Scenarios
---------

-  You need to delete unnecessary DB instances.
-  You need to delete the DB instance that fails to be created.

.. important::

   -  Deleted DB instances cannot be recovered. Exercise caution when performing this operation. To retain data, back up the data before deleting the DB instance.
   -  DB instances cannot be deleted when operations are being performed on them.
   -  If you delete a DB instance, its automated backups are also deleted and you are no longer charged for them. Manual backups are still retained and will incur additional costs.


Deleting a DB Instance
----------------------

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and a project.
#. Click **Service List**. Under **Database**, click **GaussDB**.
#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.
#. On the **Instance Management** page, locate the DB instance to be deleted and click **More** > **Delete** in the **Operation** column.
#. In the displayed dialog box, click **Yes**. Refresh the **Instance Management** page later to check that the deletion is successful.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
