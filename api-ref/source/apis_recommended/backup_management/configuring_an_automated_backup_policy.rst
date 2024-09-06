:original_name: opengauss_api_0026.html

.. _opengauss_api_0026:

Configuring an Automated Backup Policy
======================================

Function
--------

This API is used to configure an automated backup policy. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

   PUT https://{*Endpoint*}/v3/{*project_id*}/instances/{*instance_id*}/backups/policy

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

-  Parameter description

   .. table:: **Table 2** Parameter description

      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------+
      | Name            | Mandatory       | Type            | Description                                                                        |
      +=================+=================+=================+====================================================================================+
      | backup_policy   | Yes             | Object          | Backup policy information.                                                         |
      |                 |                 |                 |                                                                                    |
      |                 |                 |                 | For details, see :ref:`Table 3 <en-us_topic_0000001917290732__table365210374319>`. |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001917290732__table365210374319:

   .. table:: **Table 3** backup_policy field data structure description

      +-----------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name                  | Mandatory       | Type            | Description                                                                                                                                                                                                                                                                                                                                                         |
      +=======================+=================+=================+=====================================================================================================================================================================================================================================================================================================================================================================+
      | keep_days             | Yes             | Integer         | Backup retention days                                                                                                                                                                                                                                                                                                                                               |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | The value ranges from 1 to 732.                                                                                                                                                                                                                                                                                                                                     |
      +-----------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | start_time            | Yes             | String          | Backup time window. The creation of an automated backup will be triggered during the backup time window.                                                                                                                                                                                                                                                            |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | The value cannot be empty. It must be a valid value in the "hh:mm-HH:MM" format. The current time is in the UTC format.                                                                                                                                                                                                                                             |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | The value of **HH** must be 1 greater than the value of **hh**. The values of **mm** and **MM** must be the same and must be **00**.                                                                                                                                                                                                                                |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | Example value:                                                                                                                                                                                                                                                                                                                                                      |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | 21:00-22:00                                                                                                                                                                                                                                                                                                                                                         |
      +-----------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | period                | Yes             | String          | Full backup period. An automated full backup will be created on the specified days of the week.                                                                                                                                                                                                                                                                     |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | The value is a number separated by commas (,), indicating the days of the week. For example, **1,2,3,4** indicates that the backup period is Monday, Tuesday, Wednesday, and Thursday.                                                                                                                                                                              |
      +-----------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | differential_period   | Yes             | String          | Differential backup interval. Interval for automatic differential backup.                                                                                                                                                                                                                                                                                           |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | Its value can be **15**, **30**, **60**, **180**, **360**, **720**, or **1440**. The unit is minute.                                                                                                                                                                                                                                                                |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | Example value: 30                                                                                                                                                                                                                                                                                                                                                   |
      +-----------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | rate_limit            | No              | Integer         | Upload speed at which data is uploaded to OBS. **0 MB/s** indicates that the speed is not limited. The upload speed is related to the bandwidth.                                                                                                                                                                                                                    |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | Value range: **0**\ ``-``\ **1024**                                                                                                                                                                                                                                                                                                                                 |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | Minimum value: **0**                                                                                                                                                                                                                                                                                                                                                |
      +-----------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | prefetch_block        | No              | Integer         | Number of prefetch pages from the modified pages in the disk table file during a differential backup. When modified pages are adjacent (for example, with a bulk data load), you can set this parameter to a large value. When modified pages are scattered (for example, random update), you can set this parameter to a small value. The default value is **64**. |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | Value: **1** to **8192**                                                                                                                                                                                                                                                                                                                                            |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | Minimum value: **1**.                                                                                                                                                                                                                                                                                                                                               |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | Maximum value: **8192**                                                                                                                                                                                                                                                                                                                                             |
      +-----------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | file_split_size       | No              | Integer         | Size by which full and differential backup files are split, in GB. The value is from **0** to **1024**, but it must be a multiple of 4. The default value is **4**. **0** indicates the size is not limited.                                                                                                                                                        |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | Value range: **0**\ ``-``\ **1024**                                                                                                                                                                                                                                                                                                                                 |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | Minimum value: **0**                                                                                                                                                                                                                                                                                                                                                |
      |                       |                 |                 |                                                                                                                                                                                                                                                                                                                                                                     |
      |                       |                 |                 | Maximum value: **1024**                                                                                                                                                                                                                                                                                                                                             |
      +-----------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | filesplit_size        | No              | Integer         | Size by which full and differential backup files are split. Deprecated field. Leave it blank.                                                                                                                                                                                                                                                                       |
      +-----------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | enable_standby_backup | No              | Boolean         | Whether to enable backup on a standby node. (It is not suitable for single-node instances and instances earlier than 3.100.0.)                                                                                                                                                                                                                                      |
      +-----------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Configuring a backup policy for GaussDB (Set backup retention period to seven days and backup time window to 19:00-20:00)

.. code-block::

   {
       "backup_policy": {
           "keep_days": 7,
           "start_time": "19:00-20:00",
           "period": "1,2,3,4,5",
           "differential_period": "30",
           "rate_limit": 75 ,
           "prefetch_block": 64 ,
           "file_split_size": 4 ,
           "enable_standby_backup" : false
       }
   }

Response
--------

-  Normal response

   None

-  Example normal response

   {}

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
