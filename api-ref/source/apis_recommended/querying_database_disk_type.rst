:original_name: opengauss_newapi_0028.html

.. _opengauss_newapi_0028:

Querying Database Disk Type
===========================

Function
--------

This API is used to query the disk type of a DB instance. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

   GET https://{*Endpoint*}/v3/{project_id}/storage-type?version={version}&ha_mode={ha_mode}

-  Example

   -  Querying the disk type of a distributed DB instance

      https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/storage-type?version=1.4&ha_mode=enterprise

-  Parameter description

   .. table:: **Table 1** Parameter description

      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                                                         |
      +=================+=================+=================+=====================================================================================================================+
      | project_id      | Yes             | String          | Project ID of a tenant in a region.                                                                                 |
      |                 |                 |                 |                                                                                                                     |
      |                 |                 |                 | To obtain this value, see :ref:`Obtaining a Project ID <opengauss_api_0034>`.                                       |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------+
      | version         | Yes             | String          | DB version number. To obtain the DB version number, see :ref:`Querying DB Engine Versions <opengauss_newapi_0006>`. |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------+
      | ha_mode         | No              | String          | Instance type. The value can be **enterprise** (distributed). It is case-insensitive.                               |
      |                 |                 |                 |                                                                                                                     |
      |                 |                 |                 | Value:                                                                                                              |
      |                 |                 |                 |                                                                                                                     |
      |                 |                 |                 | -  **enterprise**                                                                                                   |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

-  Normal response

   .. table:: **Table 2** Parameter description

      +-----------------------+-----------------------+-----------------------------------------------------------------------------------+
      | Parameter             | Type                  | Description                                                                       |
      +=======================+=======================+===================================================================================+
      | storage_type          | Array of objects      | Storage type information.                                                         |
      |                       |                       |                                                                                   |
      |                       |                       | For details, see :ref:`Table 3 <en-us_topic_0000001917290656__response_storage>`. |
      +-----------------------+-----------------------+-----------------------------------------------------------------------------------+

   .. _en-us_topic_0000001917290656__response_storage:

   .. table:: **Table 3** storage_type field data structure description

      +----------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                  | Type                  | Description                                                                                                                     |
      +============================+=======================+=================================================================================================================================+
      | name                       | String                | Storage type. Its value can be:                                                                                                 |
      |                            |                       |                                                                                                                                 |
      |                            |                       | **ULTRAHIGH**: indicates the SSD.                                                                                               |
      +----------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------+
      | az_status                  | map<String, String>   | **key** indicates the AZ ID, and **value** indicates the specification status in the AZ. Its value can be any of the following: |
      |                            |                       |                                                                                                                                 |
      |                            |                       | -  **normal**: on sale.                                                                                                         |
      |                            |                       | -  **unsupported**: not supported.                                                                                              |
      |                            |                       | -  **sellout**: sold out.                                                                                                       |
      +----------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------+
      | support_compute_group_type | List<String>          | Performance specifications. Its value can be any of the following:                                                              |
      |                            |                       |                                                                                                                                 |
      |                            |                       | -  **normal**: general-enhanced                                                                                                 |
      |                            |                       | -  **normal2**: general-enhanced II                                                                                             |
      |                            |                       | -  **armFlavors**: Kunpeng general computing-plus                                                                               |
      +----------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------+

-  Example normal response

Database disk types:

.. code-block:: text


   {
       "storage_type": [
           {
               "name": "ULTRAHIGH",
               "az_status": {
                   "eu-de-01": "normal",
                   "eu-de-02": "normal",
                   "eu-de-03": "normal"
               },
               "support_compute_group_type": [
                   "normal",
                   "armFlavors",
                   "normal2"
               ]
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
