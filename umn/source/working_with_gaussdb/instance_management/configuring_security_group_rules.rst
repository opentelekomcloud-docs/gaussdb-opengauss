:original_name: opengauss_security_group.html

.. _opengauss_security_group:

Configuring Security Group Rules
================================

Scenarios
---------

A security group is a collection of access control rules for ECSs and GaussDB instances that are within the same VPC, have the same security requirements, and are mutually trusted.

If you need to change the security group when creating a distributed instance, ensure that the TCP ports in the inbound rule include the following: 40000-60480, 20050, 5000-5001, 2379-2380, 6000, 6500, and *<database port>-(<database port> + 100)*. (For example, if the database port is 8000, the TCP ports for the security group must include 8000-8100.)

If you need to change the security group when creating a primary/standby instance, ensure that the TCP ports in the inbound rule include the following: 20050, 5000-5001, 2379-2380, 6000, 6500, and *<database port>-(<database port> + 100)*. (For example, if the database port is 8000, the TCP ports for the security group must include 8000-8100.)

To ensure database security and reliability, you need to configure security group rules to allow specific IP addresses and ports to access the GaussDB instances.

When connecting to GaussDB instances over a private network, you need to configure security group rules for the GaussDB instances and associated ECSs, separately.

-  GaussDB instance: Configure an **inbound** **rule** for the security group with which the GaussDB instance is associated.
-  ECS: The default security group rule allows all outbound data packets. In this case, you do not need to configure a security rule for the ECS. When not all outbound traffic is allowed in the security group, you need to configure an **outbound rule** for the ECS.

Precautions
-----------

The default security group rule allows all outbound data packets. This means that ECSs and instances associated with the same security group can access each other by default. After a security group is created, you can add security group rules to control the access from and to the GaussDB instance.

-  By default, you can create up to 500 security group rules.
-  Ensure that each security group has no more than 50 rules.
-  To access a GaussDB instance from resources outside the security group, configure an **inbound rule** for the security group associated with the instance.
-  The group name must be unique.
-  Outbound rules typically do not apply to DB instances. The rules are used only when a DB instance acts as a client.
-  If a DB instance resides in a VPC but is not publicly accessible, you can also use a VPN connection to connect to it.

.. note::

   The default value of **Source** is **0.0.0.0/0**, indicating that all IP addresses can access the GaussDB instance as long as they are associated with the same security group as the instance.

Procedure
---------

#. Log in to the management console.
#. Click |image1| and choose **Network** > **Virtual Private Cloud**.
#. In the navigation pane on the left, choose **Security Groups**.
#. On the **Security Group** page, locate the security group and click **Manage Rule** in the **Operation** column.
#. On the displayed page, click **Add Rule**.
#. In the displayed dialog box, set required parameters to add an inbound rule.
#. Click **OK**.

.. |image1| image:: /_static/images/en-us_image_0000002088518738.png
