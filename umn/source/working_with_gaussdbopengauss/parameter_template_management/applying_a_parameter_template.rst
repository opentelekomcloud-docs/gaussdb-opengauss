:original_name: opengauss_01_0054.html

.. _opengauss_01_0054:

Applying a Parameter Template
=============================

**Scenarios**
-------------

Changes to parameters in a custom parameter template take effect for DB instances only after you apply this parameter template to target DB instances. A parameter template can be applied only to DB instances of the same version.

A parameter template can be applied to the same DB instance once only.

Procedure
---------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and a project.

#. Click **Service List**. Under **Database**, click **GaussDB**.

#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.

#. On the **Parameter Template Management** page, perform the following operations based on the type of the parameter template to be applied:

   -  If you intend to apply a default parameter template to DB instances, click **Default Templates** and locate the target parameter template. Click **Apply** in the **Operation** column.
   -  If you intend to apply a custom parameter template to DB instances, click **Custom Templates** and locate the target parameter template. Choose **More** > **Apply** in the **Operation** column.

   A parameter template can be applied to one or more DB instances.

#. In the displayed dialog box, select one or more DB instances to which the parameter template will be applied and click **OK**.

   After the parameter template is successfully applied, you can view the application records by referring to :ref:`Viewing Application Records of a Parameter Template <opengauss_01_0055>`.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
