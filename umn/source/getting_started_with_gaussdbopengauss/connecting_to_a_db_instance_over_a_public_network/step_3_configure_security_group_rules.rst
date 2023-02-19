:original_name: opengauss_01_0029.html

.. _opengauss_01_0029:

Step 3: Configure Security Group Rules
======================================

Scenarios
---------

A security group is a collection of access control rules for ECSs and GaussDB(openGauss) DB instances that have the same security protection requirements and are mutually trusted in a VPC.

To ensure database security and reliability, you need to configure security group rules to allow specific IP addresses and ports to access the GaussDB(openGauss) DB instances.

When you attempt to connect to a DB instance through an EIP, you need to configure an **inbound rule** for the security group associated with the DB instance.

Precautions
-----------

The default security group rule allows all outbound data packets. ECSs and GaussDB(openGauss) DB instances can access each other if they are deployed in the same security group. After a security group is created, you can add security group rules to control the access from and to the GaussDB(openGauss) DB instance in the security group.

-  By default, you can create up to 500 security group rules.
-  To prevent high network latency for the first packet, you are advised to create a maximum of 50 rules for each security group.
-  To access a GaussDB(openGauss) DB instance from resources outside the security group, you need to configure an **inbound rule** for the security group associated with the DB instance.

.. note::

   The default value of **Source** is **0.0.0.0/0**, indicating that all IP addresses can access the GaussDB(openGauss) DB instance in the security group.

Procedure
---------

#. Log in to the management console.
#. Under **Network**, click **Virtual Private Cloud**.
#. In the navigation pane on the left, choose **Access Control** > **Security Groups**.
#. On the **Security Groups** page, locate the target security group and click **Manage Rule** in the **Operation** column.
#. On the displayed page, click **Add Rule**.
#. In the displayed dialog box, set required parameters to add an inbound rule.
#. Click **OK**.
