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

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. On the **Parameter Templates** page, click **Custom Templates**. Locate the parameter template and click **Replicate** in the **Operation** column.

   Alternatively, on the **Instances** page, click the instance name to go to the **Basic Information** page. In the navigation pane, choose **Parameters**. On the displayed page, click **Replicate** to generate a new parameter template for future use.

#. In the displayed dialog box, configure required information and click **OK**.

   -  The template name is case-sensitive and can contain 1 to 64 characters. Only letters, digits, hyphens (-), underscores (_), and periods (.) are allowed.
   -  The template description can contain up to 256 characters, but cannot contain carriage returns and the following special characters: >!<"&'=

   After the parameter template is replicated, a new template is generated in the list on the **Custom Templates** tab of the **Parameter Templates** page.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
