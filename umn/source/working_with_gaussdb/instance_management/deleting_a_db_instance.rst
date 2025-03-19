:original_name: opengauss_01_0035.html

.. _opengauss_01_0035:

Deleting a DB Instance
======================

Scenarios
---------

-  You need to delete unnecessary DB instances.
-  You need to delete the DB instance that fails to be created.

.. important::

   -  Deleted DB instances cannot be recovered. Exercise caution when performing this operation. To retain data, back up the data before deleting a DB instance.
   -  DB instances cannot be deleted when operations are being performed on them.
   -  When instances are deleted, manual backups are retained.


Deleting a DB Instance
----------------------

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and project.
#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.
#. On the **Instances** page, locate the instance you want to delete and click **More** > **Delete** in the **Operation** column.
#. In the displayed dialog box, click **Yes**. Refresh the **Instances** page later and view that the instance has been removed from the instance list.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
