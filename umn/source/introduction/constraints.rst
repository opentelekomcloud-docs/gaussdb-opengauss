:original_name: opengauss_01_0013.html

.. _opengauss_01_0013:

Constraints
===========

To ensure the stability and security of GaussDB(openGauss), certain constraints are put in place for access or permission control. :ref:`Table 1 <opengauss_01_0013__t94dbd204cbaf49088da63b6600993a64>` describes such constraints.

.. _opengauss_01_0013__t94dbd204cbaf49088da63b6600993a64:

.. table:: **Table 1** Function constraints

   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Function Item                     | Constraints                                                                                                                                                                                     |
   +===================================+=================================================================================================================================================================================================+
   | Database access                   | -  Security group rules must be added to allow the ECSs to access GaussDB(openGauss) DB instances.                                                                                              |
   |                                   |                                                                                                                                                                                                 |
   |                                   |    By default, a GaussDB(openGauss) DB instance cannot be accessed by an ECS in a different security group. To allow it, you must add an inbound rule to the GaussDB(openGauss) security group. |
   |                                   |                                                                                                                                                                                                 |
   |                                   | -  The default port is **8000**. You can change it only when creating a DB instance.                                                                                                            |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Deployment                        | ECSs in which GaussDB(openGauss) DB instances are deployed are not directly visible to users. You can access GaussDB(openGauss) DB instances only through an IP address and a port number.      |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Database root permissions         | Only the **root** permissions are available on the instance creation page.                                                                                                                      |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Database parameter modification   | Some parameters can be modified on the console.                                                                                                                                                 |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DB instance reboot                | GaussDB(openGauss) DB instances cannot be rebooted through commands. They must be rebooted on the management console.                                                                           |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Backup files                      | GaussDB(openGauss) backup files are stored in OBS buckets and are not visible to users.                                                                                                         |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
