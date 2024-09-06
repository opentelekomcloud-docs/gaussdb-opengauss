:original_name: opengauss_newapi_0024.html

.. _opengauss_newapi_0024:

Creating a Manual Backup
========================

Function
--------

This API is used to create a manual backup. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

   POST https://{*Endpoint*}/v3/{project_id}/backups

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/backups

-  Parameter description

   .. table:: **Table 1** Parameter description

      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                   |
      +=================+=================+=================+===============================================================================+
      | project_id      | Yes             | String          | Project ID of a tenant in a region.                                           |
      |                 |                 |                 |                                                                               |
      |                 |                 |                 | To obtain this value, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+

Request
-------

-  Parameter description

   .. table:: **Table 2** Parameter description

      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                                                                                                   |
      +=================+=================+=================+===============================================================================================================================================================+
      | instance_id     | Yes             | String          | DB instance ID.                                                                                                                                               |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | name            | Yes             | String          | Backup name. It must contain 4 to 64 characters and start with a letter. Only letters (case-sensitive), digits, hyphens (-), and underscores (_) are allowed. |
      |                 |                 |                 |                                                                                                                                                               |
      |                 |                 |                 | Minimum characters: **4**                                                                                                                                     |
      |                 |                 |                 |                                                                                                                                                               |
      |                 |                 |                 | Maximum characters: **64**                                                                                                                                    |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | description     | No              | String          | Backup description. It contains up to 256 characters and cannot contain the following special characters: >!<"&'=                                             |
      |                 |                 |                 |                                                                                                                                                               |
      |                 |                 |                 | Maximum characters: **256**                                                                                                                                   |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Creating a manual full backup for a DB instance

.. code-block::

   {
     "instance_id" : "7e01ac5ac5274957ba506f3851d11d51in14",
     "name" : "backupwqwq3",
     "description" : "manual backup"
   }

Response
--------

-  Normal response

   .. table:: **Table 3** Response body parameters

      +-----------------------+-----------------------+--------------------------------------------------------------------------------------+
      | Parameter             | Type                  | Description                                                                          |
      +=======================+=======================+======================================================================================+
      | backup                | Object                | Backup information.                                                                  |
      |                       |                       |                                                                                      |
      |                       |                       | For details, see :ref:`Table 4 <en-us_topic_0000001947569849__response_backupinfo>`. |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------+
      | job_id                | String                | Task ID.                                                                             |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001947569849__response_backupinfo:

   .. table:: **Table 4** backup field data structure description

      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | Parameter             | Type                  | Description                                                                                                        |
      +=======================+=======================+====================================================================================================================+
      | id                    | String                | Backup ID.                                                                                                         |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | name                  | String                | Backup name.                                                                                                       |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | description           | String                | Backup description.                                                                                                |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | begin_time            | String                | Backup start time in the "yyyy-mm-ddThh:mm:ssZ" format.                                                            |
      |                       |                       |                                                                                                                    |
      |                       |                       | **T** is the separator between the calendar and the hourly notation of time. **Z** indicates the time zone offset. |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | status                | String                | Backup status. Value:                                                                                              |
      |                       |                       |                                                                                                                    |
      |                       |                       | -  **BUILDING**: Backup in progress                                                                                |
      |                       |                       | -  **COMPLETED**: Backup completed                                                                                 |
      |                       |                       | -  **FAILED**: Backup failed                                                                                       |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | type                  | String                | Backup type. Value: **manual** (manual full backup).                                                               |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | instance_id           | String                | DB instance ID.                                                                                                    |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+

-  Example normal response

   .. code-block:: text

      {
          "backup": {
              "id": "e76112bfb2074871bf54cb8df5af7f64br14",
              "name": "backupwqwq32",
              "description": "mannual backup",
              "status": "BUILDING",
              "type": "manual",
              "begin_time": "2022-05-09T18:02:31+0800",
              "instance_id": "fd26e3bf26e5467587eec857e4f66ef0in14"
          },
          "job_id": "e4733090-b2c8-4ea7-a33a-f55f65723fb3"
      }

-  Abnormal response

   For details, see :ref:`Abnormal Request Results <opengauss_api_0031>`.

Status Code
-----------

-  Normal

   202

-  Abnormal

   For details, see :ref:`Status Codes <opengauss_api_0032>`.

Error Code
----------

For details, see :ref:`Error Codes <opengauss_api_0033>`.
