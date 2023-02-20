:original_name: opengauss_01_0009.html

.. _opengauss_01_0009:

Permissions Management
======================

If you need to assign different permissions to employees in your enterprise to access your GaussDB(openGauss) resources, IAM is a good choice for fine-grained permissions management. IAM provides identity authentication, permissions management, and access control, helping you securely manage access to your resources.

With IAM, you can use your account to create IAM users for your employees, and assign permissions to the users to control their access to specific resource types. For example, some software developers in your enterprise need to use GaussDB(openGauss) resources but must not delete them or perform any high-risk operations. To achieve this result, you can create IAM users for the software developers and grant them only the permissions required for using GaussDB(openGauss) resources.

If your account does not need individual IAM users for permissions management, you may skip this chapter.

GaussDB(openGauss) Permissions
------------------------------

By default, new IAM users do not have permissions assigned. You need to add a user to one or more groups, and attach permissions policies or roles to these groups. Users inherit permissions from the groups to which they are added and can perform specified operations on cloud services. Currently, GaussDB(openGauss) shares the same permissions with RDS.

GaussDB(openGauss) is a project-level service deployed in specific physical regions. To assign GaussDB(openGauss) permissions to a user group, specify the scope as region-specific projects and select projects for the permissions to take effect. If **All projects** is selected, the permissions will take effect for the user group in all region-specific projects. When accessing GaussDB(openGauss), the users need to switch to a region where they have been authorized to use this service.

You can grant users permissions by using roles and policies.

-  Roles: A type of coarse-grained authorization mechanism that defines permissions related to users responsibilities. Only a limited number of service-level roles for authorization are available. When using roles to grant permissions, you need to also assign other roles on which the permissions depend to take effect. Roles are not ideal for fine-grained authorization and secure access control.

-  Policies: A fine-grained authorization mechanism that defines permissions required to perform operations on specific cloud resources under certain conditions. This mechanism allows for more flexible policy-based authorization and meets secure access control requirements. For example, you can grant IAM users only the permissions for managing a certain type of GaussDB(openGauss) resources. Most policies define permissions based on APIs.

:ref:`Table 1 <opengauss_01_0009__table1874504912120>` lists all the system-defined policies supported by GaussDB(openGauss).

.. _opengauss_01_0009__table1874504912120:

.. table:: **Table 1** System policy summary

   +------------------------+-----------------------------------+-----------------------+
   | Policy Name            | Description                       | Category              |
   +========================+===================================+=======================+
   | GaussDB FullAccess     | Full permissions for GaussDB      | System-defined policy |
   +------------------------+-----------------------------------+-----------------------+
   | GaussDB ReadOnlyAccess | Read-only permissions for GaussDB | System-defined policy |
   +------------------------+-----------------------------------+-----------------------+

:ref:`Table 2 <opengauss_01_0009__table582411494212>` lists the common operations supported by each system policy of GaussDB(openGauss). Choose proper system policies according to this table.

.. _opengauss_01_0009__table582411494212:

.. table:: **Table 2** Common operations supported by the GaussDB(openGauss) system policies

   +---------------------------------------------+--------------------+------------------------+
   | Operation                                   | GaussDB FullAccess | GaussDB ReadOnlyAccess |
   +=============================================+====================+========================+
   | Creating a GaussDB(openGauss) DB instance   | Y                  | x                      |
   +---------------------------------------------+--------------------+------------------------+
   | Deleting a GaussDB(openGauss) DB instance   | Y                  | x                      |
   +---------------------------------------------+--------------------+------------------------+
   | Querying a GaussDB(openGauss) instance list | Y                  | Y                      |
   +---------------------------------------------+--------------------+------------------------+

.. table:: **Table 3** Common operations and supported actions

   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Operation                                  | Action                              | Remarks                                                                                                                               |
   +============================================+=====================================+=======================================================================================================================================+
   | Creating a DB instance                     | gaussdb:instance:create             | To select a VPC, subnet, and security group, you need to configure the following actions:                                             |
   |                                            |                                     |                                                                                                                                       |
   |                                            | gaussdb:param:list                  | vpc:vpcs:list                                                                                                                         |
   |                                            |                                     |                                                                                                                                       |
   |                                            |                                     | vpc:vpcs:get                                                                                                                          |
   |                                            |                                     |                                                                                                                                       |
   |                                            |                                     | vpc:subnets:get                                                                                                                       |
   |                                            |                                     |                                                                                                                                       |
   |                                            |                                     | vpc:securityGroups:get                                                                                                                |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Changing DB instance specifications        | gaussdb:instance:modifySpec         | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Reboot a DB instance                       | gaussdb:instance:restart            | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Delete a DB instance                       | gaussdb:instance:delete             | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Querying a DB instance list                | gaussdb:instance:list               | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Querying DB instance details               | gaussdb:instance:list               | If the VPC, subnet, and security group are displayed in the DB instance list, you need to configure vpc:``*``:get and vpc:``*``:list. |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Changing a DB instance password            | gaussdb:instance:modify             | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Changing a database port                   | gaussdb:instance:modify             | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Changing a DB instance name                | gaussdb:instance:modify             | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Binding or unbinding an EIP                | gaussdb:instance:modify             | To display public IP addresses on the console, configure the following actions:                                                       |
   |                                            |                                     |                                                                                                                                       |
   |                                            |                                     | vpc:publicIps:get                                                                                                                     |
   |                                            |                                     |                                                                                                                                       |
   |                                            |                                     | vpc:publicIps:list                                                                                                                    |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Creating a parameter template              | gaussdb:param:create                | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Modifying a parameter template             | gaussdb:param:modify                | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Obtaining a parameter template list        | gaussdb:param:list                  | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Applying a parameter template              | gaussdb:param:apply                 | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Deleting a parameter template              | gaussdb:param:delete                | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Creating a manual backup                   | gaussdb:backup:create               | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Deleting a manual backup                   | gaussdb:backup:delete               | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Obtaining a backup list                    | gaussdb:backup:list                 | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Modifying a backup policy                  | gaussdb:instance:modifyBackupPolicy | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Deleting a manual backup                   | gaussdb:backup:delete               | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Restoring data to a new DB instance        | gaussdb:instance:create             | To select a VPC, subnet, and security group, configure the following actions:                                                         |
   |                                            |                                     |                                                                                                                                       |
   |                                            |                                     | vpc:vpcs:list                                                                                                                         |
   |                                            |                                     |                                                                                                                                       |
   |                                            |                                     | vpc:vpcs:get                                                                                                                          |
   |                                            |                                     |                                                                                                                                       |
   |                                            |                                     | vpc:subnets:get                                                                                                                       |
   |                                            |                                     |                                                                                                                                       |
   |                                            |                                     | vpc:securityGroups:get                                                                                                                |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Querying project tags                      | gaussdb:tag:list                    | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Adding or deleting project tags in batches | gaussdb:instance:dealTag            | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Modifying quotas                           | gaussdb:quota:modify                | N/A                                                                                                                                   |
   +--------------------------------------------+-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
