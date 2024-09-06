:original_name: opengauss_newapi_0026.html

.. _opengauss_newapi_0026:

Querying the Restoration Time Range
===================================

Function
--------

This API is used to query the restoration time range of an instance. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

   GET https://{*Endpoint*}/v3/{project_id}/instances/{instance_id}/restore-time?date={date}

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/instances/d2113b7c60154636b94bea1320b6a874in14/restore-time?date=2022-04-17

-  Parameter description

   .. table:: **Table 1** Parameter description

      +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                            |
      +=================+=================+=================+========================================================================================+
      | project_id      | Yes             | String          | Project ID of a tenant in a region.                                                    |
      |                 |                 |                 |                                                                                        |
      |                 |                 |                 | To obtain this value, see :ref:`Obtaining a Project ID <opengauss_api_0034>`.          |
      +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------+
      | instance_id     | Yes             | String          | Instance ID.                                                                           |
      +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------+
      | date            | Yes             | String          | Date to be queried. The value is in the "yyyy-mm-dd" format, and the time zone is UTC. |
      +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

-  Normal response

   .. table:: **Table 2** Parameter description

      +-----------------------+-----------------------+----------------------------------------------------------------------------------------+
      | Parameter             | Type                  | Description                                                                            |
      +=======================+=======================+========================================================================================+
      | restore_time          | Array of objects      | Restoration time ranges.                                                               |
      |                       |                       |                                                                                        |
      |                       |                       | For details, see :ref:`Table 3 <en-us_topic_0000001947569481__response_restore_time>`. |
      +-----------------------+-----------------------+----------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001947569481__response_restore_time:

   .. table:: **Table 3** restore_time field data structure description

      +------------+------+--------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Type | Description                                                                                                              |
      +============+======+==========================================================================================================================+
      | start_time | Long | Start time of the restoration time range in the UNIX timestamp format. The unit is millisecond and the time zone is UTC. |
      +------------+------+--------------------------------------------------------------------------------------------------------------------------+
      | end_time   | Long | End time of the restoration time range in the UNIX timestamp format. The unit is millisecond and the time zone is UTC.   |
      +------------+------+--------------------------------------------------------------------------------------------------------------------------+

-  Example normal response

.. code-block:: text

   {
       "restore_time": [
           {
               "start_time": 1652084311000,
               "end_time": 1652092839000
           },
           {
               "start_time": 1652092847000,
               "end_time": 1652094792000
           }
       ]
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
