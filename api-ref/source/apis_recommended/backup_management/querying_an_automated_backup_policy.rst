:original_name: opengauss_newapi_0022.html

.. _opengauss_newapi_0022:

Querying an Automated Backup Policy
===================================

Function
--------

This API is used to query an automated backup policy. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

   GET https://{*Endpoint*}/v3/{project_id}/instances/{instance_id}/backups/policy

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in14/backups/policy

-  Parameter description

   .. table:: **Table 1** Parameter description

      +-----------------------+-----------------------+-------------------------------------------------------------------------------+
      | Name                  | Mandatory             | Description                                                                   |
      +=======================+=======================+===============================================================================+
      | project_id            | Yes                   | Project ID of a tenant in a region.                                           |
      |                       |                       |                                                                               |
      |                       |                       | To obtain this value, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------+
      | instance_id           | Yes                   | Instance ID.                                                                  |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------+

Request
-------

None

Response
--------

-  Normal response

   .. table:: **Table 2** Parameter description

      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
      | Parameter             | Type                  | Description                                                                                |
      +=======================+=======================+============================================================================================+
      | backup_policy         | Object                | Backup policy information.                                                                 |
      |                       |                       |                                                                                            |
      |                       |                       | For details, see :ref:`Table 3 <en-us_topic_0000001947569641__response_showbackuppolicy>`. |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001947569641__response_showbackuppolicy:

   .. table:: **Table 3** backup_policy field data structure description

      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter             | Type                  | Description                                                                                                                                                                                                                                                                                                                                                         |
      +=======================+=======================+=====================================================================================================================================================================================================================================================================================================================================================================+
      | keep_days             | Integer               | Full backup retention days. Value: **1** to **732**                                                                                                                                                                                                                                                                                                                 |
      |                       |                       |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                       | Minimum value: **1**                                                                                                                                                                                                                                                                                                                                                |
      |                       |                       |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                       | Maximum value: **732**                                                                                                                                                                                                                                                                                                                                              |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | start_time            | String                | Full backup time window. The creation of an automated backup will be triggered during the backup time window. The value must be a valid value in the "hh:mm-HH:MM" format. The current time is in the UTC format.                                                                                                                                                   |
      |                       |                       |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                       | -  The **HH** value must be 1 greater than the **hh** value.                                                                                                                                                                                                                                                                                                        |
      |                       |                       | -  The values of **mm** and **MM** must be the same and must be set to **00**.                                                                                                                                                                                                                                                                                      |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | period                | String                | Full backup period. Data will be automatically backed up on the selected days every week.                                                                                                                                                                                                                                                                           |
      |                       |                       |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                       | The value is a number separated by commas (,), indicating the days of the week.                                                                                                                                                                                                                                                                                     |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | differential_period   | Integer               | Differential backup period. An automated differential backup will be performed on the specified minutes.                                                                                                                                                                                                                                                            |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | rate_limit            | Integer               | Upload speed at which data is uploaded to OBS. **0 MB/s** indicates that the speed is not limited. The upload speed is related to the bandwidth.                                                                                                                                                                                                                    |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | prefetch_block        | Integer               | Number of prefetch pages from the modified pages in the disk table file during a differential backup. When modified pages are adjacent (for example, with a bulk data load), you can set this parameter to a large value. When modified pages are scattered (for example, random update), you can set this parameter to a small value. The default value is **64**. |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | file_split_size       | Integer               | Size by which full and differential backup files are split, in GB. The value is from **0** to **1024**, but it must be a multiple of 4. The default value is **4**. **0** indicates the size is not limited.                                                                                                                                                        |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | enable_standby_backup | Boolean               | Whether to enable backup on a standby node.                                                                                                                                                                                                                                                                                                                         |
      |                       |                       |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                       | Value: **true** or **false**                                                                                                                                                                                                                                                                                                                                        |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | table_list            | Array                 | Table-level backup information. For details, see :ref:`Table 4 <en-us_topic_0000001947569641__table15883339195414>`.                                                                                                                                                                                                                                                |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001947569641__table15883339195414:

   .. table:: **Table 4** table_list field data structure description

      =========== ====== ==============
      Name        Type   Description
      =========== ====== ==============
      db_name     String Database name.
      schema_name String Schema name.
      table_name  String Table name.
      =========== ====== ==============

-  Example normal response

   Querying an automated backup policy

   .. code-block:: text

      {
          "backup_policy": {
              "period": "1,2,3,4,5,6,7",
              "keep_days": 7,
              "start_time": "18:00-19:00",
              "differential_period": 30 ,
              "rate_limit": 75 ,
              "prefetch_block": 64 ,
              "file_split_size": 4 ,
              "enable_standby_backup" : false,
              "table_list" : [ {
                   "db_name" : "table_backup_db",
                   "schema_name" : "myschema",
                   "table_name" : "table_test_1685587714722"
               } ]
          }
      }

-  Abnormal response

   For details, see :ref:`Abnormal Request Results <opengauss_api_0031>`.

Status Code
-----------

-  Normal

   200

-  Abnormal

   For details, see :ref:`Status Codes <opengauss_api_0032>`.

Error Code
----------

For details, see :ref:`Error Codes <opengauss_api_0033>`.
