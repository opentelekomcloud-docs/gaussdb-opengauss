:original_name: opengauss_01_0033.html

.. _opengauss_01_0033:

Changing an Instance Name
=========================

Scenarios
---------

You can change the name of an instance.

Constraints
-----------

You cannot perform the following operations when the instance name is being changed:

-  Binding an EIP
-  Deleting the instance
-  Creating a backup for the instance

Precautions
-----------

-  When you rename a DB instance, the metrics and events associated with the instance remain unchanged.
-  A DB instance tag is always associated with the DB instance regardless of whether the DB instance is renamed or not.
-  If a DB instance is renamed, backups of the DB instance are retained.

Procedure
---------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. On the **Instances** page, locate the instance that you want to edit name for and click |image3| next to the instance name. Then, change the name and click **OK**.

   Alternatively, click the instance name to go to the **Basic Information** page. In the **DB Information** area, click |image4| next to the **DB Instance Name** field to edit the instance name.

   The name must start with a letter and consist of 4 to 64 characters. It can contain only uppercase letters, lowercase letters, digits, hyphens (-), and underscores (_).

   -  To submit the change, click |image5|.
   -  To cancel the change, click |image6|.

#. View the new instance name.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
.. |image3| image:: /_static/images/en-us_image_0000002124277749.png
.. |image4| image:: /_static/images/en-us_image_0000002124277749.png
.. |image5| image:: /_static/images/en-us_image_0000002124277745.png
.. |image6| image:: /_static/images/en-us_image_0000002088678002.png
