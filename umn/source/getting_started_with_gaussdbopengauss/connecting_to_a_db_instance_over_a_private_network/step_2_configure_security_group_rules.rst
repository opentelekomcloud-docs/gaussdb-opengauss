:original_name: opengauss_01_0023.html

.. _opengauss_01_0023:

Step 2: Configure Security Group Rules
======================================

Scenarios
---------

A security group is a collection of access control rules for ECSs and GaussDB(openGauss) DB instances that are within the same VPC, have the same security requirements, and are mutually trusted.

To ensure database security and reliability, you need to configure security group rules to allow specific IP addresses and ports to access the GaussDB(openGauss) DB instances.

When connecting to DB instances over a private network, you need to configure security group rules for DB instances and associated ECSs, separately.

-  DB instance: Configure an **inbound** **rule** for the security group with which the DB instance is associated.
-  ECS: The default security group rule allows all outbound data packets. In this case, you do not need to configure a security rule for the ECS. When not all outbound traffic is allowed in the security group, you need to configure an **outbound rule** for the ECS.

Precautions
-----------

The default security group rule allows all outbound data packets. This means that ECSs and GaussDB(openGauss) DB instances associated with the same security group can access each other by default. You can add rules to security groups to control inbound and outbound traffic for your GaussDB(openGauss) DB instance. By associating an instance with a security group, you apply all the rules in this security group to your DB instance.

-  By default, you can add a maximum of 500 security group rules.
-  To prevent high network latency for the first packet, you are advised to create a maximum of 50 rules for each security group.
-  To access a GaussDB(openGauss) DB instance from resources outside the security group, you need to configure an **inbound rule** for the security group associated with the DB instance.

.. note::

   The default value of **Source** is **0.0.0.0/0**, indicating that all IP addresses can access the GaussDB(openGauss) DB instance as long as they are associated with the same security group as the DB instance.

Procedure
---------

#. Log in to the management console.
#. Under **Network**, click **Virtual Private Cloud**.
#. In the navigation pane on the left, choose **Access Control** > **Security Groups**.
#. On the **Security Groups** page, locate the target security group and click **Manage Rule** in the **Operation** column.
#. On the displayed page, click **Add Rule**.
#. In the displayed dialog box, set required parameters to add an inbound rule.
#. Click **OK**.
