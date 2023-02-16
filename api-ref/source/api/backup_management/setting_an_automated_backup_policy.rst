:original_name: opengauss_api_0026.html

.. _opengauss_api_0026:

Setting an Automated Backup Policy
==================================

Function
--------

This API is used to set an automated backup policy.

-  Before calling an API, you need to understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Before calling this API, obtain the required `region and endpoint <https://docs.otc.t-systems.com/en-us/endpoint/index.html>`__.

URI
---

-  URI format

   PUT https://{*Endpoint*}/opengauss/v3/{*project_id*}/instances/{*instance_id*}/backups/policy

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/opengauss/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in14/backups/policy

   -  Parameter description

      .. table:: **Table 1** Parameter description

         +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+
         | Name                  | Mandatory             | Description                                                                                             |
         +=======================+=======================+=========================================================================================================+
         | project_id            | Yes                   | Specifies the project ID of a tenant in a region.                                                       |
         |                       |                       |                                                                                                         |
         |                       |                       | For details about how to obtain the project ID, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
         +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+
         | instance_id           | Yes                   | Specifies the DB instance ID, which is compliant with the UUID format.                                  |
         +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+

Request
-------

-  Parameter description

   .. table:: **Table 2** Parameter description

      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------+
      | Name            | Mandatory       | Type            | Description                                                              |
      +=================+=================+=================+==========================================================================+
      | backup_policy   | Yes             | Object          | Specifies the backup policy information.                                 |
      |                 |                 |                 |                                                                          |
      |                 |                 |                 | For details, see :ref:`Table 3 <opengauss_api_0026__table365210374319>`. |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------+

   .. _opengauss_api_0026__table365210374319:

   .. table:: **Table 3** backup_policy field data structure description

      +---------------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name                | Mandatory       | Type            | Description                                                                                                                                                                            |
      +=====================+=================+=================+========================================================================================================================================================================================+
      | keep_days           | Yes             | Integer         | Specifies the backup retention days.                                                                                                                                                   |
      |                     |                 |                 |                                                                                                                                                                                        |
      |                     |                 |                 | The value ranges from 1 to 732.                                                                                                                                                        |
      +---------------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | start_time          | Yes             | String          | Specifies the backup time window. Automated backups will be triggered during the backup time window.                                                                                   |
      |                     |                 |                 |                                                                                                                                                                                        |
      |                     |                 |                 | The value cannot be empty. It must be a valid value in the "hh:mm-HH:MM" format. The current time is in the UTC format.                                                                |
      |                     |                 |                 |                                                                                                                                                                                        |
      |                     |                 |                 | The value of **HH** must be 1 greater than the value of **hh**. The values of **mm** and **MM** must be the same and must be **00**.                                                   |
      |                     |                 |                 |                                                                                                                                                                                        |
      |                     |                 |                 | Example value:                                                                                                                                                                         |
      |                     |                 |                 |                                                                                                                                                                                        |
      |                     |                 |                 | 21:00-22:00                                                                                                                                                                            |
      +---------------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | period              | Yes             | String          | Specifies the full backup period. An automated full backup will be performed on the specified days of the week.                                                                        |
      |                     |                 |                 |                                                                                                                                                                                        |
      |                     |                 |                 | The value is a number separated by commas (,), indicating the days of the week. For example, **1,2,3,4** indicates that the backup period is Monday, Tuesday, Wednesday, and Thursday. |
      +---------------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | differential_period | Yes             | String          | Specifies the interval for automated differential backups.                                                                                                                             |
      |                     |                 |                 |                                                                                                                                                                                        |
      |                     |                 |                 | The value is 15, 30, 60, 180, 360, 720, or 1440 in minute.                                                                                                                             |
      |                     |                 |                 |                                                                                                                                                                                        |
      |                     |                 |                 | Example value: 30                                                                                                                                                                      |
      +---------------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  Example request

   .. code-block:: text

      {
          "backup_policy": {
              "keep_days": 7,
              "start_time": "19:00-20:00",
              "period": "1,2,3,4,5",
              "differential_period": "30"
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

Status Codes
------------

For details, see :ref:`Status Codes <opengauss_api_0032>`.

Error Codes
-----------

For details, see :ref:`Error Codes <opengauss_api_0033>`.
