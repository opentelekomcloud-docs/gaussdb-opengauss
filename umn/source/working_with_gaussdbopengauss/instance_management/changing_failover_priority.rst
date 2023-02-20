:original_name: opengauss_01_0045.html

.. _opengauss_01_0045:

Changing Failover Priority
==========================

Scenarios
---------

You can change the failover priority of a DB instance on the **Basic Information** page.

Procedure
---------

#. Click **Service List**. Under **Database**, click **GaussDB**.
#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.
#. Click the target DB instance that you want to change its failover priority. The **Basic Information** page is displayed.
#. In the **DB Information** area, click **Change** in the **Failover Priority** field.
#. In the displayed dialog box, select **Reliability** or **Availability** as required.

   -  **Reliability**: Data consistency is given priority during a failover. This is recommended for users with highest priority for data consistency.
   -  **Availability**: Database availability is given priority during a failover. This is recommended for users who require their databases to provide uninterrupted online services.

   .. important::

      -  In availability scenarios, contact technical support if you want to change the following database parameters:

         -  **recovery_time_target**: If this parameter is incorrectly changed, the DB instance will undergo forcible failovers frequently.
         -  **audit_system_object**: If this parameter is incorrectly changed, DDL audit logs will be lost.

      -  You need to manually reboot the DB instance for the change to take effect.
      -  Failover priority cannot be changed when the DB instance is rebooting.

#. Click **OK**.
