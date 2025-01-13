:original_name: opengauss_01_0048.html

.. _opengauss_01_0048:

Modifying Parameters in a Parameter Template
============================================

You can modify parameters in a custom parameter template to bring out the best performance of the service.

You can change parameter values in custom parameter templates only and cannot change parameter values in default parameter templates.

If you change a parameter value, the time when the change takes effect is determined by the parameter type.

To modify the parameters of a parameter template, go to the **Parameter Templates** page and under the **Custom Templates** tab, click the template name, change its parameter values and save the changes. Then, you can apply the changed parameter template to instances. The dynamic parameter changes take effect immediately, and the static parameter changes take effect only after you manually reboot the instances.

.. note::

   GaussDB has default parameter templates whose parameter values cannot be changed. You can view these parameter values by clicking the default parameter template names. If a custom parameter template is configured incorrectly and applied to instances, the instances maybe cannot be rebooted. If this happens, you can refer to the settings used by a default parameter template.

Modifying Parameter Template Parameters
---------------------------------------

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and project.
#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.
#. Choose **Parameter Templates** in the navigation pane on the left. On the **Custom Templates** page, click the parameter template name.
#. Modify parameters as needed.

   -  To save your changes, click **Save**.
   -  To cancel your changes, click **Cancel**.
   -  To preview your changes, click **Preview**.

#. After the parameters are modified, click **Change History** to view what changes have been made.

   .. important::

      The changes take effect only after you apply the parameter template to instances. For details, see :ref:`Applying a Parameter Template <opengauss_01_0054>`.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
