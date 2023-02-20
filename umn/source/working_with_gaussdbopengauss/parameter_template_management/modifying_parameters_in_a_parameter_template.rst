:original_name: opengauss_01_0048.html

.. _opengauss_01_0048:

Modifying Parameters in a Parameter Template
============================================

This section guides you on how to edit parameters in the parameter template that you have created to meet your service requirements and achieve optimal performance.

You can change parameter values in custom parameter templates only and cannot change parameter values in default parameter templates.

If you change a parameter value, when the change takes effect is determined by the type of parameter.

When you change parameter values in parameter templates in batches on the **Parameter Template Management** page and save the changes, the changes will take effect only after you apply the parameter templates to DB instances. If you modify static parameters in a custom parameter template on the **Parameter Template Management** page and save the changes, the changes take effect only after you apply the parameter template to DB instances and manually reboot the DB instances.

.. note::

   GaussDB(openGauss) has default parameter templates whose parameter values cannot be changed. You can view these parameter values by clicking the default parameter templates. If a custom parameter template is set incorrectly and causes a database reboot to fail, you can configure the custom parameter template by referring to the configurations of the default parameter template.

Modifying Parameters in Batches
-------------------------------

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and a project.
#. Click **Service List**. Under **Database**, click **GaussDB**.
#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.
#. Choose **Parameter Template Management** in the navigation pane on the left. On the **Custom Templates** page, click the target parameter template.
#. Modify the parameters as required.

   -  To save the modifications, click **Save**.
   -  To cancel the modifications, click **Cancel**.
   -  To preview the modifications, click **Preview**.

#. After the parameters are modified, you can click **Change History** to view the modification details.

   .. important::

      The modifications take effect only after you apply the parameter template to DB instances. For details, see :ref:`Applying a Parameter Template <opengauss_01_0054>`.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
