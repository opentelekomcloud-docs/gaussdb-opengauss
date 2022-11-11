:original_name: opengauss_api_0015.html

.. _opengauss_api_0015:

Creating a DB Instance
======================

Function
--------

This API is used to create a GaussDB(openGauss) DB instance. GaussDB(openGauss) supports distributed instances.

-  Before calling an API, you need to understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Before calling this API, obtain the required `region and endpoint <https://docs.otc.t-systems.com/en-us/endpoint/index.html>`__.

URI
---

-  URI format

   POST https://{*Endpoint*}/opengauss/v3/{project_id}/instances

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/opengauss/v3/0483b6b16e954cb88930a360d2c4e663/instances

-  Parameter description

   .. table:: **Table 1** Parameter description

      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+
      | Name                  | Mandatory             | Description                                                                                             |
      +=======================+=======================+=========================================================================================================+
      | project_id            | Yes                   | Specifies the project ID of a tenant in a region.                                                       |
      |                       |                       |                                                                                                         |
      |                       |                       | For details about how to obtain the project ID, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Parameter description (for distributed instance creation)

   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name              | Mandatory       | Type            | Description                                                                                                                                                                                                                                                                  |
   +===================+=================+=================+==============================================================================================================================================================================================================================================================================+
   | name              | Yes             | String          | Specifies the DB instance name.                                                                                                                                                                                                                                              |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | DB instances of the same type can have same names under the same tenant.                                                                                                                                                                                                     |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | The value consists of 4 to 64 characters and starts with a letter. It is case-sensitive and contains only letters, digits, hyphens (-), and underscores (_).                                                                                                                 |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | datastore         | Yes             | Object          | Specifies the database information.                                                                                                                                                                                                                                          |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | For details, see :ref:`Table 3 <opengauss_api_0015__en-us_topic_0128427213_table64243102>`.                                                                                                                                                                                  |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ha                | Yes             | Object          | Used to create a distributed instance. For details, see :ref:`Table 4 <opengauss_api_0015__tb0e86cf38fcb4f5aa2916c48e56eff25>`.                                                                                                                                              |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | configuration_id  | No              | String          | Specifies the parameter template ID. If this parameter is not specified, the default parameter template is used.                                                                                                                                                             |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | port              | No              | String          | Specifies the database port.                                                                                                                                                                                                                                                 |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | The GaussDB(openGauss) database port ranges from 1024 to 39998 (excluding the following which are occupied by the system and cannot be used: 2378, 2379, 2380, 4999, 5000, 5999, 6000, 6001, 8097, 8098, 12016, 12017, 20049, 20050, 21731, 21732, 32122, 32123, and 32124). |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | If this parameter is not specified, the default port **8000** is used.                                                                                                                                                                                                       |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password          | Yes             | String          | Specifies the database password.                                                                                                                                                                                                                                             |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | The password cannot be empty and must contain 8 to 32 characters, including at least three of the following: uppercase letters, lowercase letters, digits, and special characters (``~!@#%^*-_=+?,``).                                                                       |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | You are advised to enter a strong password to improve security and prevent security risks such as brute force cracking.                                                                                                                                                      |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | backup_strategy   | No              | Object          | Specifies the backup policy.                                                                                                                                                                                                                                                 |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | For details, see :ref:`Table 5 <opengauss_api_0015__t1903dd23671e4892bff3e308239cbf2f>`.                                                                                                                                                                                     |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | flavor_ref        | Yes             | String          | Specifies the specification code. The value cannot be empty. For details on how to obtain the GaussDB(openGauss) specification code, see :ref:`Table 1 <opengauss_api_0037__ted9b9d433c8a4c52884e199e17f94479>`.                                                             |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | volume            | Yes             | Object          | Specifies the volume information.                                                                                                                                                                                                                                            |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | For details, see :ref:`Table 6 <opengauss_api_0015__en-us_topic_0128427213_table10656503>`.                                                                                                                                                                                  |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region            | Yes             | String          | Specifies the region ID.                                                                                                                                                                                                                                                     |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | The value cannot be empty. For details about how to obtain this parameter value, see `Regions and Endpoints <https://docs.otc.t-systems.com/en-us/endpoint/index.html>`__.                                                                                                   |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | availability_zone | Yes             | String          | Specifies the AZ ID.                                                                                                                                                                                                                                                         |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | The value cannot be empty. You can deploy GaussDB(openGauss) in the same AZ or across three different AZs, and use commas (,) to separate AZs. For example:                                                                                                                  |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | -  To deploy GaussDB(openGauss) in the same AZ, enter three same AZ IDs.                                                                                                                                                                                                     |
   |                   |                 |                 | -  To deploy GaussDB(openGauss) across three different AZs, enter three different AZ IDs.                                                                                                                                                                                    |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | The value cannot be empty. For details about how to obtain this parameter value, see `Regions and Endpoints <https://docs.otc.t-systems.com/en-us/endpoint/index.html>`__.                                                                                                   |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | vpc_id            | Yes             | String          | Specifies the VPC ID. To obtain this parameter value, use either of the following methods:                                                                                                                                                                                   |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | -  Method 1: Log in to VPC console and view the VPC ID in the VPC details.                                                                                                                                                                                                   |
   |                   |                 |                 | -  Method 2: See the "Querying VPCs" section in the *Virtual Private Cloud API Reference*.                                                                                                                                                                                   |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | subnet_id         | Yes             | String          | Specifies the network ID. To obtain this parameter value, use either of the following methods:                                                                                                                                                                               |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | -  Method 1: Log in to VPC console and click the target subnet on the **Subnets** page. You can view the network ID on the displayed page.                                                                                                                                   |
   |                   |                 |                 | -  Method 2: See the "Querying Subnets" section under "APIs" or the "Querying Networks" section under "OpenStack Neutron APIs" in *Virtual Private Cloud API Reference*.                                                                                                     |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | security_group_id | No              | String          | Specifies the security group which the DB instance belongs to. To obtain this parameter value, use either of the following methods:                                                                                                                                          |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | -  Method 1: Log in to VPC console. Choose **Access Control** > **Security Groups** in the navigation pane on the left. On the displayed page, click the target security group. You can view the security group ID on the displayed page.                                    |
   |                   |                 |                 | -  Method 2: See the "Querying Security Groups" section in the *Virtual Private Cloud API Reference*.                                                                                                                                                                        |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | charge_info       | No              | Object          | Specifies the billing type, which is only pay-per-use.                                                                                                                                                                                                                       |
   |                   |                 |                 |                                                                                                                                                                                                                                                                              |
   |                   |                 |                 | For details, see :ref:`Table 7 <opengauss_api_0015__t762e388e6ab14a2fb53abf847470ee7b>`.                                                                                                                                                                                     |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sharding_num      | Yes             | Integer         | Specifies the number of shards. The value ranges from 1 to 9.                                                                                                                                                                                                                |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | coordinator_num   | Yes             | Integer         | Specifies the number of CNs. The value ranges from 1 to 9. The number of CNs must be less than or equal to twice the number of shards.                                                                                                                                       |
   +-------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _opengauss_api_0015__en-us_topic_0128427213_table64243102:

.. table:: **Table 3** datastore field data structure description

   +-----------------+-----------------+-----------------+---------------------------------------+
   | Name            | Mandatory       | Type            | Description                           |
   +=================+=================+=================+=======================================+
   | type            | Yes             | String          | Specifies the DB engine. Value:       |
   |                 |                 |                 |                                       |
   |                 |                 |                 | GaussDB(openGauss).                   |
   +-----------------+-----------------+-----------------+---------------------------------------+
   | version         | Yes             | String          | Specifies the database version.       |
   |                 |                 |                 |                                       |
   |                 |                 |                 | The following versions are supported: |
   |                 |                 |                 |                                       |
   |                 |                 |                 | -  1.1                                |
   |                 |                 |                 | -  1.2                                |
   +-----------------+-----------------+-----------------+---------------------------------------+

.. _opengauss_api_0015__tb0e86cf38fcb4f5aa2916c48e56eff25:

.. table:: **Table 4** ha field data structure description

   +------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------+
   | Name             | Mandatory       | Type            | Description                                                                                                   |
   +==================+=================+=================+===============================================================================================================+
   | mode             | Yes             | String          | Specifies the distributed mode of GaussDB(openGauss). The value is **enterprise** and is case insensitive.    |
   +------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------+
   | replication_mode | Yes             | String          | Specifies the replication mode for the standby node.                                                          |
   |                  |                 |                 |                                                                                                               |
   |                  |                 |                 | Value:                                                                                                        |
   |                  |                 |                 |                                                                                                               |
   |                  |                 |                 | For GaussDB(openGauss), the value is **sync**.                                                                |
   |                  |                 |                 |                                                                                                               |
   |                  |                 |                 | .. note::                                                                                                     |
   |                  |                 |                 |                                                                                                               |
   |                  |                 |                 |    -  **sync** indicates the synchronous replication mode.                                                    |
   +------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------+
   | consistency      | Yes             | String          | Specifies the instance consistency type. The value can be **strong** or **eventual** and is case-insensitive. |
   +------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------+

.. _opengauss_api_0015__t1903dd23671e4892bff3e308239cbf2f:

.. table:: **Table 5** backup_strategy field data structure description

   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------+
   | Name            | Mandatory       | Type            | Description                                                                                                                     |
   +=================+=================+=================+=================================================================================================================================+
   | start_time      | Yes             | String          | Specifies the backup time window. Automated backups will be triggered during the backup time window.                            |
   |                 |                 |                 |                                                                                                                                 |
   |                 |                 |                 | The value cannot be empty. It must be a valid value in the "hh:mm-HH:MM" format. The current time is in the UTC format.         |
   |                 |                 |                 |                                                                                                                                 |
   |                 |                 |                 | -  The **HH** value must be 1 greater than the **hh** value.                                                                    |
   |                 |                 |                 | -  The values of **mm** and **MM** must be the same and must be set to any of the following: **00**, **15**, **30**, or **45**. |
   |                 |                 |                 |                                                                                                                                 |
   |                 |                 |                 | Example value:                                                                                                                  |
   |                 |                 |                 |                                                                                                                                 |
   |                 |                 |                 | -  08:15-09:15                                                                                                                  |
   |                 |                 |                 | -  23:00-00:00                                                                                                                  |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------+
   | keep_days       | No              | Integer         | Specifies the retention days for specific backup files.                                                                         |
   |                 |                 |                 |                                                                                                                                 |
   |                 |                 |                 | The value ranges from 0 to 732. If this parameter is not specified or set to **0**, the automated backup policy is disabled.    |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------+

.. _opengauss_api_0015__en-us_topic_0128427213_table10656503:

.. table:: **Table 6** volume field data structure description

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name            | Mandatory       | Type            | Description                                                                                                                                                                  |
   +=================+=================+=================+==============================================================================================================================================================================+
   | type            | Yes             | String          | Specifies the volume type.                                                                                                                                                   |
   |                 |                 |                 |                                                                                                                                                                              |
   |                 |                 |                 | The value can only be **ULTRAHIGH** (case-sensitive), indicating SSD.                                                                                                        |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | size            | Yes             | Integer         | Specifies the volume size.                                                                                                                                                   |
   |                 |                 |                 |                                                                                                                                                                              |
   |                 |                 |                 | The value must be a multiple of the number of shards multiplied by 40 GB, ranging from the number of shards multiplied by 40 GB to the number of shards multiplied by 16 TB. |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _opengauss_api_0015__t762e388e6ab14a2fb53abf847470ee7b:

.. table:: **Table 7** chargeInfo field data structure description

   +-------------+-----------+--------+----------------------------------------------------------------------------------------------------------+
   | Name        | Mandatory | Type   | Description                                                                                              |
   +=============+===========+========+==========================================================================================================+
   | charge_mode | Yes       | String | Specifies the billing mode. The value can only be **postPaid**, indicating the pay-per-use billing mode. |
   +-------------+-----------+--------+----------------------------------------------------------------------------------------------------------+

-  Request example

.. code-block:: text

   Creating a DB instance of the enterprise edition:
   {
       "name": "user1-v3-independent",
       "datastore": {
           "type": "GaussDB(openGauss)",
           "version": "1.1"
       },
       "flavor_ref": "gaussdb.opengauss.ee.dn.m6.2xlarge.8.in",
       "volume": {
           "type": "ULTRAHIGH",
           "size": 120
       },
       "region": "eu-de",
        "availability_zone": "eu-de-01,eu-de-01,eu-de-01",
       "vpc_id": "1f011c32-2de2-4aa8-a161-9498dbcef329",
       "subnet_id": "54a44bec-e36f-441e-86bb-d749ace9c189",
       "security_group_id": "c6123999-8532-421c-9db6-e078013ff58f",
       "backup_strategy": {
           "start_time": "17:00-18:00",
           "keep_days": 7
       },
       "charge_info": {
           "charge_mode": "postPaid"
       },
       "password": "Gauss_234",
       "configuration_id": "b000d7c91f1749da87315700793a11d4pr14",
       "time_zone": "UTC+08:00",
       "ha":{
           "mode":"enterprise",
           "consistency":"strong",
           "replication_mode":"sync"
       },
       "sharding_num":1,
       "coordinator_num":1,
       "port":8000
   }

Response
--------

-  Normal response

   .. table:: **Table 8** Parameter description

      +-----------------------+-----------------------+------------------------------------------------------------------------------------------+
      | Name                  | Type                  | Description                                                                              |
      +=======================+=======================+==========================================================================================+
      | instance              | Object                | Indicates the DB instance information.                                                   |
      |                       |                       |                                                                                          |
      |                       |                       | For details, see :ref:`Table 9 <opengauss_api_0015__tf299c2e624d84a8b844d4c90e2fe56c7>`. |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------+
      | job_id                | String                | Indicates the ID of the DB instance creation task.                                       |
      |                       |                       |                                                                                          |
      |                       |                       | This parameter is returned only for the creation of pay-per-use DB instances.            |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------+

   .. _opengauss_api_0015__tf299c2e624d84a8b844d4c90e2fe56c7:

   .. table:: **Table 9** instance field data structure description

      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name                  | Type                  | Description                                                                                                                                                                                                      |
      +=======================+=======================+==================================================================================================================================================================================================================+
      | id                    | String                | Indicates the DB instance ID.                                                                                                                                                                                    |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | name                  | String                | Indicates the DB instance name. DB instances of the same type can have same names under the same tenant.                                                                                                         |
      |                       |                       |                                                                                                                                                                                                                  |
      |                       |                       | The value must be 4 to 64 characters in length and start with a letter. It is case-insensitive and contains only letters, digits, hyphens (-), and underscores (_).                                              |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | status                | String                | Indicates the DB instance status. For example, **BUILD** indicates that the DB instance is being created.                                                                                                        |
      |                       |                       |                                                                                                                                                                                                                  |
      |                       |                       | Returned only for the creation of pay-per-use DB instances.                                                                                                                                                      |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | datastore             | Object                | Indicates the database information.                                                                                                                                                                              |
      |                       |                       |                                                                                                                                                                                                                  |
      |                       |                       | For details, see :ref:`Table 10 <opengauss_api_0015__te1d9101d066a4ae79c239bac67bda39f>`.                                                                                                                        |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | ha                    | Object                | Returned when a distributed instance is created.                                                                                                                                                                 |
      |                       |                       |                                                                                                                                                                                                                  |
      |                       |                       | For details, see :ref:`Table 11 <opengauss_api_0015__t4e8dd7dfd6d0454a98881bf130b3ceb8>`.                                                                                                                        |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | port                  | String                | Indicates the database port, which is the same as the request parameter.                                                                                                                                         |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | backup_strategy       | Object                | Indicates the automated backup policy.                                                                                                                                                                           |
      |                       |                       |                                                                                                                                                                                                                  |
      |                       |                       | For details, see :ref:`Table 12 <opengauss_api_0015__t2fa91fb97869490c93b3d505498959c6>`.                                                                                                                        |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | flavor_ref            | String                | Indicates the specification code. The value cannot be empty. For details on how to obtain the GaussDB(openGauss) specification code, see :ref:`Table 1 <opengauss_api_0037__ted9b9d433c8a4c52884e199e17f94479>`. |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | volume                | Object                | Indicates the volume information.                                                                                                                                                                                |
      |                       |                       |                                                                                                                                                                                                                  |
      |                       |                       | For details, see :ref:`Table 13 <opengauss_api_0015__tbf29fe1d75444bf685eceeaa4a50e457>`.                                                                                                                        |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | region                | String                | Indicates the region ID.                                                                                                                                                                                         |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | availability_zone     | String                | Indicates the AZ ID.                                                                                                                                                                                             |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | vpc_id                | String                | Indicates the VPC ID.                                                                                                                                                                                            |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | subnet_id             | String                | Indicates the network ID of the subnet.                                                                                                                                                                          |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | security_group_id     | String                | Indicates the security group to which the DB instance belongs.                                                                                                                                                   |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | charge_info           | Object                | Indicates the payment mode. Only pay-per-use is supported.                                                                                                                                                       |
      |                       |                       |                                                                                                                                                                                                                  |
      |                       |                       | For details, see :ref:`Table 14 <opengauss_api_0015__tf9b2616079124827a0c38c3651abf4e0>`.                                                                                                                        |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. _opengauss_api_0015__te1d9101d066a4ae79c239bac67bda39f:

   .. table:: **Table 10** datastore field data structure description

      +-----------------------+-----------------------+---------------------------------+
      | Name                  | Type                  | Description                     |
      +=======================+=======================+=================================+
      | type                  | String                | Indicates the DB engine. Value: |
      |                       |                       |                                 |
      |                       |                       | GaussDB(openGauss)              |
      +-----------------------+-----------------------+---------------------------------+
      | version               | String                | Indicates the database version. |
      +-----------------------+-----------------------+---------------------------------+

   .. _opengauss_api_0015__t4e8dd7dfd6d0454a98881bf130b3ceb8:

   .. table:: **Table 11** ha field data structure description

      +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Name                  | Type                  | Description                                                                                                                                   |
      +=======================+=======================+===============================================================================================================================================+
      | mode                  | String                | Indicates the distributed model. The returned value is **Enterprise**. Currently, only the distributed model is supported.                    |
      +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | replication_mode      | String                | Indicates the replication mode for the standby node.                                                                                          |
      |                       |                       |                                                                                                                                               |
      |                       |                       | Value:                                                                                                                                        |
      |                       |                       |                                                                                                                                               |
      |                       |                       | -  For GaussDB(openGauss), the value is **sync**.                                                                                             |
      |                       |                       |                                                                                                                                               |
      |                       |                       | .. note::                                                                                                                                     |
      |                       |                       |                                                                                                                                               |
      |                       |                       |    -  **sync** indicates the synchronous replication mode.                                                                                    |
      +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | consistency           | String                | Indicates the GaussDB(openGauss) reserved parameter, referring to the instance consistency type. The value can be **strong** or **eventual**. |
      +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+

   .. _opengauss_api_0015__t2fa91fb97869490c93b3d505498959c6:

   .. table:: **Table 12** backup_strategy field data structure description

      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------+
      | Name                  | Type                  | Description                                                                                                                     |
      +=======================+=======================+=================================================================================================================================+
      | start_time            | String                | Indicates the backup time window. Automated backups will be triggered during the backup time window.                            |
      |                       |                       |                                                                                                                                 |
      |                       |                       | The value cannot be empty. It must be a valid value in the "hh:mm-HH:MM" format. The current time is in the UTC format.         |
      |                       |                       |                                                                                                                                 |
      |                       |                       | -  The **HH** value must be 1 greater than the **hh** value.                                                                    |
      |                       |                       | -  The values of **mm** and **MM** must be the same and must be set to any of the following: **00**, **15**, **30**, or **45**. |
      |                       |                       |                                                                                                                                 |
      |                       |                       | Example value:                                                                                                                  |
      |                       |                       |                                                                                                                                 |
      |                       |                       | -  08:15-09:15                                                                                                                  |
      |                       |                       | -  23:00-00:00                                                                                                                  |
      |                       |                       |                                                                                                                                 |
      |                       |                       | If **backup_strategy** in the request body is empty, **02:00-03:00** is returned for **start_time** by default.                 |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------+
      | keep_days             | Integer               | Indicates the retention days for specific backup files.                                                                         |
      |                       |                       |                                                                                                                                 |
      |                       |                       | The value ranges from 0 to 732. If this parameter is not specified or set to **0**, the automated backup policy is disabled.    |
      |                       |                       |                                                                                                                                 |
      |                       |                       | If **backup_strategy** in the request body is empty, **7** is returned for **keep_days** by default.                            |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------+

   .. _opengauss_api_0015__tbf29fe1d75444bf685eceeaa4a50e457:

   .. table:: **Table 13** volume field data structure description

      +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name                  | Type                  | Description                                                                                                                                                                                                                         |
      +=======================+=======================+=====================================================================================================================================================================================================================================+
      | type                  | String                | Indicates the volume type.                                                                                                                                                                                                          |
      |                       |                       |                                                                                                                                                                                                                                     |
      |                       |                       | Its value can be any of the following and is case-sensitive:                                                                                                                                                                        |
      |                       |                       |                                                                                                                                                                                                                                     |
      |                       |                       | -  **ULTRAHIGH**: indicates the SSD type.                                                                                                                                                                                           |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | size                  | Integer               | Indicates the volume size.                                                                                                                                                                                                          |
      |                       |                       |                                                                                                                                                                                                                                     |
      |                       |                       | When creating a distributed instance, you need to specify the size to be a multiple of the number of shards multiplied by 40 GB, ranging from the number of shards multiplied by 40 GB to the number of shards multiplied by 16 TB. |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. _opengauss_api_0015__tf9b2616079124827a0c38c3651abf4e0:

   .. table:: **Table 14** chargeInfo field data structure description

      +-------------+--------+----------------------------------------------------------+
      | Name        | Type   | Description                                              |
      +=============+========+==========================================================+
      | charge_mode | String | Indicates the billing information, which is pay-per-use. |
      +-------------+--------+----------------------------------------------------------+

-  Example normal response

   .. code-block:: text

      Creating a DB instance of the enterprise edition:
      {
          "instance": {
              "id": "ad8cd1440aa94a02ae4580fcbebb3143in14",
              "name": "user1-v3-independent",
              "status": "BUILD",
              "datastore": {
                  "type": "GaussDB(openGauss)",
                  "version": "1.1"
              },
              "ha": {
                  "mode": "Enterprise",
                  "replication_mode": "sync",
                  "consistency": "strong"
              },
              "port": "8000",
              "volume": {
                  "type": "ULTRAHIGH",
                  "size": 120
              },
          "region": "eu-de",
              "backup_strategy": {
                  "start_time": "17:00-18:00",
                  "keep_days": 7
              },
              "flavor_ref": "gaussdb.opengauss.ee.dn.m6.2xlarge.8.in",
          "availability_zone": "eu-de-01,eu-de-01,eu-de-01",
              "vpc_id": "1f011c32-2de2-4aa8-a161-9498dbcef329",
              "subnet_id": "54a44bec-e36f-441e-86bb-d749ace9c189",
              "security_group_id": "c6123999-8532-421c-9db6-e078013ff58f",
              "charge_info": {
                  "charge_mode": "postPaid"
              }
          },
          "job_id": "30f2790a-a5b6-4a13-a5ab-733c746609af"
      }

-  Abnormal response

   For details, see :ref:`Abnormal Request Results <opengauss_api_0031>`.

Status Codes
------------

For details, see :ref:`Status Codes <opengauss_api_0032>`.

Error Codes
-----------

For details, see :ref:`Error Codes <opengauss_api_0033>`.
