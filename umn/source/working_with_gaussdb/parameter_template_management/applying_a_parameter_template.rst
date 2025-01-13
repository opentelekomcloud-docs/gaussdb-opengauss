:original_name: opengauss_01_0054.html

.. _opengauss_01_0054:

Applying a Parameter Template
=============================

**Scenarios**
-------------

Changes to parameters in a custom parameter template take effect for DB instances only after you apply this parameter template to target DB instances. A parameter template can be applied only to DB instances of the same version.

Procedure
---------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. On the **Parameter Templates** page, perform the following operations based on the type of the parameter template to be applied:

   -  If you intend to apply a default parameter template to DB instances, click **Default Templates** and locate the parameter template. Click **Apply** in the **Operation** column.
   -  If you intend to apply a custom parameter template to DB instances, click **Custom Templates** and locate the parameter template. Choose **More** > **Apply** in the **Operation** column.

   A parameter template can be applied to one or more DB instances.

#. In the displayed dialog box, select one or more DB instances to which the parameter template will be applied and click **OK**.

   After the parameter template is applied, you can check the application status by referring to :ref:`Viewing Application Records of a Parameter Template <opengauss_01_0055>`. If the application status of instances is **Applying**, the instances are not displayed in the instance list when you apply a parameter template again.

   .. note::

      After the parameter template is successfully applied, if you modify parameters in the parameter template and the instance status is **Parameter change. Pending reboot**, you must reboot the DB instance for the modifications to take effect. If the instance status is **Available**, the modifications take effect immediately.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
