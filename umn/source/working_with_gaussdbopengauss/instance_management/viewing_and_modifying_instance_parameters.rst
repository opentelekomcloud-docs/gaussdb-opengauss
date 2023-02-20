:original_name: opengauss_01_0042.html

.. _opengauss_01_0042:

Viewing and Modifying Instance Parameters
=========================================

This section describes how to modify the GaussDB(openGauss) DB instance parameters in real time, or view the parameter values of the current DB instance.

**Procedure**
-------------

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and a project.
#. Click **Service List**. Under **Database**, click **GaussDB**.
#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.
#. On the **Instance Management** page, click the target DB instance.
#. In the navigation pane on the left, click **Parameters** to go to the parameter modification page.

   -  You can modify and query the parameters applied to the target DB instance on this page. Click **Preview** to confirm the modifications or **Cancel** to cancel the modifications. After the modifications are confirmed, click **Save**.

      .. note::

         Check the value in the **Effective upon Reboot** column.

         -  If the value is **Yes** and the DB instance status on the **Instance Management** page is **Pending reboot**, you must reboot the DB instance for the modifications to take effect.
         -  If the value is **No**, the modifications take effect immediately.

   -  You can also click **Export** to export the parameters of the target DB instance to a custom template or to a file.
   -  You can click **Compare** to compare the parameter template applied to the target DB instance with an existing parameter template.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
