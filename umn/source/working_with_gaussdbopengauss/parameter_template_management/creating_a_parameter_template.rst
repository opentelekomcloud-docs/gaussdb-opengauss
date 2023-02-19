:original_name: opengauss_01_0047.html

.. _opengauss_01_0047:

Creating a Parameter Template
=============================

You can use database parameter templates to manage the DB engine configuration. A database parameter template acts as a container for engine configuration values that can be applied to one or more DB instances.

When you create a DB instance, the system will automatically allocate a default database parameter template for you, not allowing you to select parameter templates. This default template contains DB engine defaults and system defaults based on the engine, compute class, and allocated storage of the DB instance. You cannot modify the parameter settings of a default parameter template. You must create your own parameter template to change parameter settings.

.. important::

   Not all DB engine parameters can be changed in a custom parameter template.

If you want to use your custom parameter template, create a parameter template and select it when you create a DB instance or apply it to an existing DB instance following the instructions provided in :ref:`Applying a Parameter Template <opengauss_01_0054>`.

When you have already created a parameter template and want to include most of the custom parameters and values from that template in a new parameter template, you can replicate that parameter template following the instructions provided in :ref:`Replicating a Parameter Template <opengauss_01_0052>`.

The following are the key points you should know when using parameters in a parameter template:

-  When you change a parameter value in a parameter template and save the change, the change applies only to current DB instance and does not affect the other DB instances.
-  When you change a dynamic parameter value in a parameter template and save the change, the change takes effect immediately. When you change a static parameter value in a parameter template and save the change, the change will take effect only after you manually reboot the DB instances to which the parameter template applies.
-  Improperly setting parameters in a parameter template may have unintended adverse effects, including degraded performance and system instability. Exercise caution when modifying database parameters and you need to back up data before modifying parameters in a parameter template. Before applying parameter template changes to a production DB instance, you should try out these changes on a test DB instance.

.. note::

   GaussDB(openGauss) parameter template quotas are not shared by DDS.

   By default, each user can create a maximum of 100 parameter templates for DB instances. All GaussDB engines share the parameter template quota.

Procedure
---------

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and a project.
#. Click **Service List**. Under **Database**, click **GaussDB**.
#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.
#. In the navigation pane on the left, choose **Parameter Template Management**.
#. On the **Parameter Template Management** page, click **Create Parameter Template**.
#. In the displayed dialog box, configure required information and click **OK**.

   -  Select a DB engine for the parameter template.
   -  The template name must consist of 1 to 64 characters. It can contain only uppercase letters, lowercase letters, digits, hyphens (-), underscores (_), and periods (.).
   -  The description consists of a maximum of 256 characters and cannot contain the carriage return character or the following special characters: >!<"&'=

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
