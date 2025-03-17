:original_name: opengauss_newapi_0003.html

.. _opengauss_newapi_0003:

Obtaining Parameter Templates
=============================

Function
--------

This API is used to obtain parameter templates, including all databases' default and custom parameter templates. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

   GET https://{*Endpoint*}/v3/{project_id}/configurations?offset={offset}&limit={limit}

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/configurations?offset=1&limit=3

-  Parameter description

   .. table:: **Table 1** Parameter description

      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                                                                                                                                                                   |
      +=================+=================+=================+===============================================================================================================================================================================================================================================================================================================================================================+
      | project_id      | Yes             | String          | Project ID of a tenant in a region.                                                                                                                                                                                                                                                                                                                           |
      |                 |                 |                 |                                                                                                                                                                                                                                                                                                                                                               |
      |                 |                 |                 | To obtain this value, see :ref:`Obtaining a Project ID <opengauss_api_0034>`.                                                                                                                                                                                                                                                                                 |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | offset          | No              | Integer         | Index offset. If **offset** is set to *N*, the resource query starts from the N+1 piece of data. The default value is **0**, indicating that the query starts from the first piece of data. The value cannot be a negative number. For example, if this parameter is set to **0** and **limit** is set to **10**, only the 1st to 10th records are displayed. |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | limit           | No              | Integer         | Number of records to be queried. The default value is **100**. The value cannot be a negative number. The minimum value is **1** and the maximum value is **100**. For example, if this parameter is set to **10**, a maximum of 10 records can be displayed.                                                                                                 |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

-  Normal response

   .. table:: **Table 2** Parameter description

      +----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------+
      | Parameter      | Type             | Description                                                                                                                    |
      +================+==================+================================================================================================================================+
      | configurations | Array of objects | Parameter template information. For details, see :ref:`Table 3 <en-us_topic_0000001947569465__response_configurationsummary>`. |
      +----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------+
      | count          | Integer          | Total number of records.                                                                                                       |
      +----------------+------------------+--------------------------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001947569465__response_configurationsummary:

   .. table:: **Table 3** configurations field data structure description

      +-----------------------+-----------------------+-------------------------------------------------------------+
      | Parameter             | Type                  | Description                                                 |
      +=======================+=======================+=============================================================+
      | id                    | String                | Parameter template ID.                                      |
      +-----------------------+-----------------------+-------------------------------------------------------------+
      | name                  | String                | Parameter template name.                                    |
      +-----------------------+-----------------------+-------------------------------------------------------------+
      | description           | String                | Parameter template description.                             |
      +-----------------------+-----------------------+-------------------------------------------------------------+
      | datastore_version     | String                | Engine version.                                             |
      +-----------------------+-----------------------+-------------------------------------------------------------+
      | datastore_name        | String                | Engine name.                                                |
      +-----------------------+-----------------------+-------------------------------------------------------------+
      | ha_mode               | String                | Instance type.                                              |
      +-----------------------+-----------------------+-------------------------------------------------------------+
      | created               | String                | Creation time in the "yyyy-MM-dd HH:mm:ss" format.          |
      +-----------------------+-----------------------+-------------------------------------------------------------+
      | updated               | String                | Update time in the "yyyy-MM-dd HH:mm:ss" format.            |
      +-----------------------+-----------------------+-------------------------------------------------------------+
      | user_defined          | Boolean               | Whether the parameter template is a custom template.        |
      |                       |                       |                                                             |
      |                       |                       | -  **false**: The parameter template is a default template. |
      |                       |                       | -  **true**: The parameter template is a custom template.   |
      +-----------------------+-----------------------+-------------------------------------------------------------+

-  Example normal response

   Parameter templates:

   .. code-block:: text

      {
          "count": 3,
          "configurations": [
              {
                  "id": "b000d7c91f1749da87315700793a11d4pr14",
                  "name": "Default-Enterprise-Edition-GaussDB-1.0-INDEP",
                  "description": "Default parameter template for Enterprise Edition GaussDB 1.0-Independent",
                  "created": "2022-03-23 07:20:11",
                  "updated": "2022-03-23 07:20:11",
                  "datastore_version": "1.0",
                  "datastore_name": "GaussDB(for openGauss)",
                  "ha_mode": "enterprise",
                  "user_defined": false
              },
              {
                  "id": "8d99f260ea1b4493a1b349e7abce5c09pr14",
                  "name": "Default-Enterprise-Edition-GaussDB-1.1-INDEP",
                  "description": "Default parameter template for Enterprise Edition GaussDB 1.1-Independent",
                  "created": "2022-03-23 07:20:11",
                  "updated": "2022-03-23 07:20:11",
                  "datastore_version": "1.1",
                  "datastore_name": "GaussDB(for openGauss)",
                  "ha_mode": "enterprise",
                  "user_defined": false
              },
              {
                  "id": "0f44b65521a8414d8b8811df810d94ccpr14",
                  "name": "Default-Enterprise-Edition-GaussDB-1.2-INDEP",
                  "description": "Default parameter template for Enterprise Edition GaussDB 1.2-Independent",
                  "created": "2022-03-23 07:20:11",
                  "updated": "2022-03-23 07:20:11",
                  "datastore_version": "1.2",
                  "datastore_name": "GaussDB(for openGauss)",
                  "ha_mode": "enterprise",
                  "user_defined": false
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
