:original_name: opengauss_newapi_0037.html

.. _opengauss_newapi_0037:

Modifying the Recycling Policy
==============================

Function
--------

This API is used to modify the recycling policy. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

   PUT https://{*Endpoint*}/v3/{project_id}/recycle-policy

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0611f1bd8b00d5d32f17c017f15b599f/recycle-policy

-  Parameter description

   .. table:: **Table 1** Parameter description

      +-----------------------+-----------------------+-------------------------------------------------------------------------------+
      | Name                  | Mandatory             | Description                                                                   |
      +=======================+=======================+===============================================================================+
      | project_id            | Yes                   | Project ID of a tenant in a region.                                           |
      |                       |                       |                                                                               |
      |                       |                       | To obtain this value, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------+

Request
-------

-  Parameter description

   .. table:: **Table 2** Parameter description

      +----------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------+
      | Name           | Mandatory | Type   | Description                                                                                                                 |
      +================+===========+========+=============================================================================================================================+
      | recycle_policy | Yes       | Object | Recycling policy. For details, see :ref:`Table 3 <en-us_topic_0000001917290436__en-us_topic_0151958051_table477719232522>`. |
      +----------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001917290436__en-us_topic_0151958051_table477719232522:

   .. table:: **Table 3** recycle_policy field data structure description

      +--------------------------+-----------------+-----------------+------------------------------------+
      | Name                     | Mandatory       | Type            | Description                        |
      +==========================+=================+=================+====================================+
      | retention_period_in_days | Yes             | Integer         | Deleted instance retention period. |
      |                          |                 |                 |                                    |
      |                          |                 |                 | Value range: **1** to **7**.       |
      +--------------------------+-----------------+-----------------+------------------------------------+

Example Request
---------------

Setting the retention period of deleted instances to 5 days

.. code-block::

   {
       "recycle_policy": {
           "retention_period_in_days": 5
       }
   }

Response
--------

-  Normal response

   .. table:: **Table 4** Parameter description

      +--------+--------+---------------------------------------------------------------------------------+
      | Name   | Type   | Description                                                                     |
      +========+========+=================================================================================+
      | result | String | Modification result. **success** indicates that the modification is successful. |
      +--------+--------+---------------------------------------------------------------------------------+

-  Example normal response

   .. code-block::

      {
          "result": "success"
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
