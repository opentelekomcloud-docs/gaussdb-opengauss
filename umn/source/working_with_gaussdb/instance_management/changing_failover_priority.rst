:original_name: opengauss_01_0045.html

.. _opengauss_01_0045:

Changing Failover Priority
==========================

Scenarios
---------

GaussDB provides failover priority on availability or reliability. You can change the failover priority of a DB instance on the **Basic Information** page.

Procedure
---------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. Click the name of the target instance to go to the **Basic Information** page.

#. In the **DB Information** area, click **Change** in the **Failover Priority** field.

#. In the displayed dialog box, select **Reliability** or **Availability** as required.

   -  **Reliability**: Data consistency is given priority during a failover. This is recommended for applications with highest priority for data consistency.
   -  **Availability**: Database availability is given priority during a failover. This is recommended for applications that require their databases to provide uninterrupted online services.

   .. important::

      If **Availability** is selected, exercise caution when changing the following database parameters:

      -  **recovery_time_target**: If this parameter is incorrectly changed, the instance will undergo frequent forced failovers. To change this parameter, contact technical support.
      -  **audit_system_object**: If this parameter is incorrectly changed, DDL audit logs will be lost. To change this parameter, contact technical support.

#. Click **OK**.

#. After you change some parameters, manually reboot the instance for the changes to take effect. For details, see :ref:`Rebooting a DB Instance <opengauss_01_0034>`.

   The failover priority cannot be changed when the DB instance is in the **Rebooting** state.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
