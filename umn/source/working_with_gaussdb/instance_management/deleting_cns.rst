:original_name: opengauss_01_0128.html

.. _opengauss_01_0128:

Deleting CNs
============

Scenarios
---------

As service demand decreases, some CNs are left idle. To improve resource utilization, you can delete unnecessary coordinator nodes of your GaussDB instances.

Precautions
-----------

-  Deleting CNs does not interrupt ongoing services.
-  You can only delete the CNs of instances that were deployed independently.
-  At least one CN needs to be reserved for each DB instance.
-  Before deleting a CN, ensure that the CN is not in a JDBC connection configuration, or the high availability of the JDBC connection may be affected.
-  DDL operations will be rolled back when CNs are being deleted.
-  PITR backup is suspended when CNs are being deleted and is automatically restored after deletion is complete.
-  After the deletion is complete, a full backup is performed automatically.
-  Before you delete CNs, you need to ensure that the instance status and all CNs are normal.
-  If the CN to be deleted is CN_5001, the system will randomly select another CN to delete.

Procedure
---------

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and project.
#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.
#. On the **Instances** page, click the name of the instance for which you want to delete CNs.
#. In the **DB Information** area of the **Basic Information** page, delete CNs.

   a. Click **Delete** next to **Coordinator Nodes**.

   b. Select the coordinator nodes to be deleted.

   c. Click **Next**.

   d. Confirm the information about the CNs to be deleted and click **Submit**.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
