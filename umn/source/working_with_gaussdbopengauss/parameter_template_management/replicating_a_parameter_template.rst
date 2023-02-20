:original_name: opengauss_01_0052.html

.. _opengauss_01_0052:

Replicating a Parameter Template
================================

**Scenarios**
-------------

You can replicate a parameter template you have created. When you have already created a parameter template and want to include most of the custom parameters and values from that template in a new parameter template, you can replicate that parameter template.

After a parameter template is replicated, the new template may be displayed about 5 minutes later.

Default parameter templates cannot be replicated. You can create parameter templates based on the default ones.

Procedure
---------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and a project.

#. Click **Service List**. Under **Database**, click **GaussDB**.

#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.

#. On the **Parameter Template Management** page, click **Custom Templates**. Locate the target parameter template and click **Replicate** in the **Operation** column.

   Alternatively, click the target DB instance on the **Instance Management** page. On the **Parameters** page, click **Export** to generate a new parameter template for future use.

#. In the displayed dialog box, configure required information and click **OK**.

   -  The template name must consist of 1 to 64 characters. It can contain only uppercase letters, lowercase letters, digits, hyphens (-), underscores (_), and periods (.).
   -  The description consists of a maximum of 256 characters and cannot contain the carriage return character or the following special characters: >!<"&'=

   After the parameter template is replicated, a new template is generated in the list in the **Customer Templates** tab of the **Parameter Template Management** page.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
