:original_name: opengauss_01_0026.html

.. _opengauss_01_0026:

Overview
========

This section describes how to create a GaussDB(openGauss) DB instance on the management console and bind an EIP to the DB instance to make the instance accessible via the Internet.

If you are using GaussDB(openGauss) for the first time, see the constraints described in :ref:`Constraints <opengauss_01_0013>`.

Process
-------

:ref:`Figure 1 <opengauss_01_0026__fig138110377499>` illustrates the process of connecting to a DB instance over a public network.

.. _opengauss_01_0026__fig138110377499:

.. figure:: /_static/images/en-us_image_0000001072030787.png
   :alt: **Figure 1** Connecting to a DB instance over a public network

   **Figure 1** Connecting to a DB instance over a public network

-  :ref:`Step 1: Create a DB instance. <opengauss_01_0027>` Confirm the specifications, storage, network, and database account configurations of the GaussDB(openGauss) DB instance based on service requirements.
-  :ref:`Step 2: Bind an EIP. <opengauss_01_0028>` The Elastic IP service provides independent public IP addresses and bandwidth for Internet access. You can apply for an EIP on the VPC console and bind the EIP to the GaussDB(openGauss) DB instance.
-  :ref:`Step 3: Configure security group rules. <opengauss_01_0029>` To access a GaussDB(openGauss) DB instance from resources outside the security group, you need to configure an **inbound rule** for the security group associated with the DB instance.
-  :ref:`Step 4: Connect to a DB instance through gsql <opengauss_01_0030>`. Connect to a GaussDB(openGauss) DB instance using the command-line tool gsql.
