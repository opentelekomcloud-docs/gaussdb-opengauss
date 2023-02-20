:original_name: opengauss_01_0021.html

.. _opengauss_01_0021:

Overview
========

This section describes how to create a GaussDB(openGauss) DB instance on the management console and connect to the DB instance through an ECS.

If you are using GaussDB(openGauss) for the first time, see the constraints described in :ref:`Constraints <opengauss_01_0013>`.

Process
-------

:ref:`Figure 1 <opengauss_01_0021__fig1863662694619>` illustrates the process of connecting to a GaussDB(openGauss) DB instance over a private network.

.. _opengauss_01_0021__fig1863662694619:

.. figure:: /_static/images/en-us_image_0000001072358981.png
   :alt: **Figure 1** Connecting to a GaussDB(openGauss) DB instance over a private network

   **Figure 1** Connecting to a GaussDB(openGauss) DB instance over a private network

-  :ref:`Step 1: Create a DB instance. <opengauss_01_0022>` Confirm the specifications, storage, network, and database account configurations of the GaussDB(openGauss) DB instance based on service requirements.
-  :ref:`Step 2: Configure security group rules. <opengauss_01_0023>`

   -  If the ECS and DB instance are in the same security group, they can communicate with each other by default. No security group rule needs to be configured. Go to :ref:`Step 3: Connect to a DB Instance Through gsql <opengauss_01_0024>`.
   -  If the ECS and DB instance are in different security groups, you need to configure security group rules for them, separately.

      -  DB instance: Configure an **inbound rule** for the security group with which the DB instance is associated.
      -  ECS: The default security group rule allows all outbound data packets. In this case, you do not need to configure a security rule for the ECS. When not all outbound traffic is allowed in the security group, you need to configure an **outbound rule** for the ECS to allow all outbound packets.

-  :ref:`Step 3: Connect to a DB instance through gsql. <opengauss_01_0024>` Connect to a GaussDB(openGauss) DB instance using the command-line tool gsql.
