:original_name: opengauss_01_0047.html

.. _opengauss_01_0047:

Creating a Parameter Template
=============================

You can use parameters in a parameter template to manage DB engine configurations. A parameter template can be applied to one or more instances.

If you create a DB instance without specifying a custom parameter template, a default parameter template is used. This default template contains DB engine defaults and system defaults based on the engine, compute specifications, and allocated storage of the instance. Default parameter templates cannot be modified, but you can create your own parameter template to change parameter settings.

.. important::

   Not all DB engine parameters can be changed in a custom parameter template.

If you want to use your custom parameter template, create a parameter template and select it when you create a DB instance or apply it to an existing DB instance by following the instructions provided in :ref:`Applying a Parameter Template <opengauss_01_0054>`.

When you have already created a parameter template and want to include most of the custom parameters and values from that template in a new parameter template, you can replicate that parameter template following the instructions provided in :ref:`Replicating a Parameter Template <opengauss_01_0052>`.

The following are the key points you should know when using parameters in a parameter template:

-  In the **Parameters** page, when you change a parameter value in a parameter template and save the change, the change applies only to current instance and does not affect the other instances.
-  Some modifications take effect only after you manually reboot the DB instance.
-  Improperly setting parameters in a parameter template may have unintended adverse effects, including degraded performance and system instability. Exercise caution when changing database parameters and you need to back up data before changing parameters in a parameter template. Do not perform the boundary testing in the parameter template, or the instance will be abnormal. Before applying parameter template changes to a production DB instance, you should try out these changes on a test DB instance.

.. note::

   GaussDB parameter template quotas are not shared by DDS.

   By default, there are up to 100 parameter templates for instances in each project. All GaussDB engines share the parameter template quota.

Procedure
---------

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and project.
#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.
#. In the navigation pane on the left, choose **Parameter Templates**.
#. On the **Parameter Templates** page, click **Create Parameter Template**.
#. In the displayed dialog box, configure required information and click **OK**.

   -  Select a DB engine for the parameter template.
   -  The template name must consist of 1 to 64 characters. It can contain only uppercase letters, lowercase letters, digits, hyphens (-), underscores (_), and periods (.).
   -  The description can contain up to 256 characters, but cannot contain carriage returns and special characters ``(!<"='>&).``

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
