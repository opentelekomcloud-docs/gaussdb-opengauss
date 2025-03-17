:original_name: opengauss_newapi_0035.html

.. _opengauss_newapi_0035:

Obtaining Task Information
==========================

Function
--------

This API is used to obtain information about a task with a specified ID. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

   GET https://{*Endpoint*}/v3/{project_id}/jobs?id={id}

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0549b4a43100d4f32f51c01c2fe4acdb/jobs?id=5cbb8a90-2253-4cff-8a13-49aa8f31dfb5

Request
-------

Parameter description

.. table:: **Table 1** Request parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
   | Name            | Type            | Mandatory       | Description                                                                   |
   +=================+=================+=================+===============================================================================+
   | project_id      | string          | Yes             | Project ID of a tenant in a region.                                           |
   |                 |                 |                 |                                                                               |
   |                 |                 |                 | To obtain this value, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
   | id              | string          | Yes             | Task ID. Currently, the following tasks are supported:                        |
   |                 |                 |                 |                                                                               |
   |                 |                 |                 | -  **CreateGaussDBV5Instance**: Creating a DB instance                        |
   |                 |                 |                 | -  **BackupSnapshotGaussDBV5InInstance**: Creating a manual backup            |
   |                 |                 |                 | -  **CloneGaussDBV5NewInstance**: Restoring data to a new DB instance         |
   |                 |                 |                 | -  **RestoreGaussDBV5InInstance**: Restoring data to the original DB instance |
   |                 |                 |                 | -  **DeleteGaussDBV5Instance**: Deleting a DB instance                        |
   |                 |                 |                 | -  **EnlargeGaussDBV5Volume**: Scaling up storage                             |
   |                 |                 |                 | -  **ResizeGaussDBV5Flavor**: Changing specifications                         |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+

Response
--------

-  Normal response

   .. table:: **Table 2** Parameter description

      +------+--------+--------------------------------------------------------------------------------------------------------+
      | Name | Type   | Description                                                                                            |
      +======+========+========================================================================================================+
      | job  | Object | Task information. For details, see :ref:`Table 3 <en-us_topic_0000001917130832__table54571314103317>`. |
      +------+--------+--------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001917130832__table54571314103317:

   .. table:: **Table 3** job field data structure description

      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name                  | Type                  | Description                                                                                                                                                                    |
      +=======================+=======================+================================================================================================================================================================================+
      | id                    | String                | Task ID.                                                                                                                                                                       |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | name                  | String                | Task name.                                                                                                                                                                     |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | status                | String                | Task execution status                                                                                                                                                          |
      |                       |                       |                                                                                                                                                                                |
      |                       |                       | Value:                                                                                                                                                                         |
      |                       |                       |                                                                                                                                                                                |
      |                       |                       | -  **Running**: The task is being executed.                                                                                                                                    |
      |                       |                       | -  **Completed**: The task is successfully executed.                                                                                                                           |
      |                       |                       | -  **Failed**: The task fails to be executed.                                                                                                                                  |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | created               | String                | Creation time in the "yyyy-mm-ddThh:mm:ssZ" format.                                                                                                                            |
      |                       |                       |                                                                                                                                                                                |
      |                       |                       | **T** is the separator between the calendar and the hourly notation of time. **Z** indicates the time zone offset.                                                             |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | ended                 | String                | End time in the "yyyy-mm-ddThh:mm:ssZ" format.                                                                                                                                 |
      |                       |                       |                                                                                                                                                                                |
      |                       |                       | **T** is the separator between the calendar and the hourly notation of time. **Z** indicates the time zone offset.                                                             |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | progress              | String                | Task execution progress.                                                                                                                                                       |
      |                       |                       |                                                                                                                                                                                |
      |                       |                       | .. note::                                                                                                                                                                      |
      |                       |                       |                                                                                                                                                                                |
      |                       |                       |    The execution progress (such as **"60%"**, indicating the task execution progress is 60%) is displayed only when the task is being executed. Otherwise, **""** is returned. |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | instance              | Object                | Instance on which the task is executed.                                                                                                                                        |
      |                       |                       |                                                                                                                                                                                |
      |                       |                       | For details, see :ref:`Table 4 <en-us_topic_0000001917130832__table4062895917262>`.                                                                                            |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | fail_reason           | String                | Task failure information.                                                                                                                                                      |
      +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001917130832__table4062895917262:

   .. table:: **Table 4** instance field data structure description

      ==== ====== =================
      Name Type   Description
      ==== ====== =================
      id   String DB instance ID.
      name String DB instance name.
      ==== ====== =================

-  Example normal response

   Parameter templates:

   .. code-block:: text

      {
        "job" : {
          "id" : "5cbb8a90-2253-4cff-8a13-49aa8f31dfb5",
          "name" : "CreateGaussDBV5Instance",
          "status" : "Completed",
          "created" : "2021-07-12T09:22:04+0800",
          "ended" : "2021-07-12T10:10:13+0800",
          "progress" : "",
          "instance" : {
            "id" : "b34f8c791f2643578510c093aa2351a8in14",
            "name" : "gauss-c1a3"
          },
          "fail_reason" : null
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
