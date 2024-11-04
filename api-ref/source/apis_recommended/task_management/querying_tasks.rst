:original_name: opengauss_newapi_0072.html

.. _opengauss_newapi_0072:

Querying Tasks
==============

Function
--------

This API is used to query the tasks in the task center. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

Constraints
-----------

The tasks of the last month can be queried at most.

URI
---

-  URI format

   GET https://{*Endpoint*}/v3/{project_id}/tasks

-  Example

   -  Querying running tasks

      https://gaussdb.eu-de.otc.t-systems.com/v3/0611f1bd8b00d5d32f17c017f15b599f/tasks?status=Running&name=CreateGaussDBV5Instance&offset=1&limit=10

   -  Querying completed tasks

      https://gaussdb.eu-de.otc.t-systems.com/v3/0611f1bd8b00d5d32f17c017f15b599f/tasks?status=Completed&name=CreateGaussDBV5Instance&offset=1&limit=10

   -  Querying failed tasks

      https://gaussdb.eu-de.otc.t-systems.com/v3/0611f1bd8b00d5d32f17c017f15b599f/tasks?status=Failed&name=CreateGaussDBV5Instance&offset=1&limit=10

-  Parameter description

   .. table:: **Table 1** Parameter description

      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name            | Mandatory       | Type            | Description                                                                                                                                                                                                                        |
      +=================+=================+=================+====================================================================================================================================================================================================================================+
      | project_id      | Yes             | String          | Project ID of a tenant in a region.                                                                                                                                                                                                |
      |                 |                 |                 |                                                                                                                                                                                                                                    |
      |                 |                 |                 | To obtain this value, see :ref:`Obtaining a Project ID <opengauss_api_0034>`.                                                                                                                                                      |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | status          | No              | String          | Task status. Value:                                                                                                                                                                                                                |
      |                 |                 |                 |                                                                                                                                                                                                                                    |
      |                 |                 |                 | -  **Running**                                                                                                                                                                                                                     |
      |                 |                 |                 | -  **Completed**                                                                                                                                                                                                                   |
      |                 |                 |                 | -  **Failed**                                                                                                                                                                                                                      |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | name            | No              | String          | Task name.                                                                                                                                                                                                                         |
      |                 |                 |                 |                                                                                                                                                                                                                                    |
      |                 |                 |                 | -  **CreateGaussDBV5Instance**: Creating a DB instance                                                                                                                                                                             |
      |                 |                 |                 | -  **BackupSnapshotGaussDBV5InInstance**: Creating a manual backup                                                                                                                                                                 |
      |                 |                 |                 | -  **CloneGaussDBV5NewInstance**: Restoring data to a new DB instance                                                                                                                                                              |
      |                 |                 |                 | -  **RestoreGaussDBV5InInstance**: Restoring data to the original DB instance                                                                                                                                                      |
      |                 |                 |                 | -  **RestoreGaussDBV5InInstanceToExistedInst**: Restoring data to an existing DB instance                                                                                                                                          |
      |                 |                 |                 | -  **DeleteGaussDBV5Instance**: Deleting a DB instance                                                                                                                                                                             |
      |                 |                 |                 | -  **EnlargeGaussDBV5Volume**: Scaling up storage                                                                                                                                                                                  |
      |                 |                 |                 | -  **ResizeGaussDBV5Flavor**: Changing specifications                                                                                                                                                                              |
      |                 |                 |                 | -  **GaussDBV5ExpandClusterCN**: Adding coordinator nodes                                                                                                                                                                          |
      |                 |                 |                 | -  **GaussDBV5ExpandClusterDN**: Adding shards                                                                                                                                                                                     |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | start_time      | No              | String          | Start time. The value is a UNIX timestamp, in milliseconds. The time zone is UTC.                                                                                                                                                  |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | end_time        | No              | String          | End time. The value is a UNIX timestamp, in milliseconds. The time zone is UTC.                                                                                                                                                    |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | offset          | No              | Integer         | Index offset. If **offset** is set to *N*, the resource query starts from the N+1 piece of data. The default value is **0**, indicating that the query starts from the first piece of data. The value cannot be a negative number. |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | limit           | No              | Integer         | Number of records to be queried. The default value is **100**. The value cannot be a negative number. The minimum value is **1** and the maximum value is **100**.                                                                 |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

-  Normal response

   .. table:: **Table 2** Parameter description

      +-----------------------+-----------------------+-----------------------------------------------------------------------------------+
      | Name                  | Type                  | Description                                                                       |
      +=======================+=======================+===================================================================================+
      | tasks                 | Array of objects      | Task list.                                                                        |
      |                       |                       |                                                                                   |
      |                       |                       | For details, see :ref:`Table 3 <en-us_topic_0000001947569857__table27316396223>`. |
      +-----------------------+-----------------------+-----------------------------------------------------------------------------------+
      | total_count           | Integer               | Number of tasks.                                                                  |
      +-----------------------+-----------------------+-----------------------------------------------------------------------------------+

   .. _en-us_topic_0000001947569857__table27316396223:

   .. table:: **Table 3** tasks field data structure description

      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | Name                  | Type                  | Description                                                                                                        |
      +=======================+=======================+====================================================================================================================+
      | instance_info         | Object                | Information about the instance associated with the task.                                                           |
      |                       |                       |                                                                                                                    |
      |                       |                       | For details, see :ref:`Table 4 <en-us_topic_0000001947569857__table1126382493011>`.                                |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | job_id                | String                | Task ID.                                                                                                           |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | name                  | String                | Task name.                                                                                                         |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | status                | String                | Task status.                                                                                                       |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | process               | String                | Task progress, in percentage (%)                                                                                   |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | fail_reason           | String                | Failure cause.                                                                                                     |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | created_at            | String                | Creation time in the "yyyy-mm-ddThh:mm:ssZ" format.                                                                |
      |                       |                       |                                                                                                                    |
      |                       |                       | **T** is the separator between the calendar and the hourly notation of time. **Z** indicates the time zone offset. |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+
      | ended_at              | String                | End time in the "yyyy-mm-ddThh:mm:ssZ" format.                                                                     |
      |                       |                       |                                                                                                                    |
      |                       |                       | **T** is the separator between the calendar and the hourly notation of time. **Z** indicates the time zone offset. |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001947569857__table1126382493011:

   .. table:: **Table 4** instance_info field data structure description

      =============== ====== =================
      Name            Type   Description
      =============== ====== =================
      instance_id     String DB instance ID.
      instance_name   String DB instance name.
      instance_status String Instance status.
      =============== ====== =================

-  Example normal response

   .. code-block::

      {
        "tasks" : [ {
          "instance_info" : {
            "instance_id" : "ce2dce50f365430abe161bab79495a6ein14",
            "instance_name" : "gauss-6568-zzh",
            "instance_status" : "creating"
          },
          "job_id" : "03bc055a-135c-4245-8bd8-b0bc6d3350b3",
          "name" : "CreateGaussDBV5Instance",
          "status" : "Failed",
          "process" : null,
          "fail_reason" : "An exception occurs when the ECS processes services.",
          "created_at": "2022-08-05T08:15:07+0800",
          "ended_at": "2022-08-09T03:06:52+0800"
        }, {
          "instance_info" : {
            "instance_id" : "20ba433bd7ee40da9cf35064f04f9e4cin14",
            "instance_name" : "gauss-7875-lt-m",
            "instance_status" : "deleted"
          },
          "job_id" : "2cc16e0b-75ab-4a28-9453-16517e990bba",
          "name" : "DeleteGaussDBV5Instance",
          "status" : "Completed",
          "process" : null,
          "fail_reason" : null,
          "created_at": "2022-08-06T09:15:07+0800",
          "ended_at": "2022-08-10T03:06:52+0800"
        } ],
        "total_count" : 2
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
