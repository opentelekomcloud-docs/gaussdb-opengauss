:original_name: opengauss_newapi_0025.html

.. _opengauss_newapi_0025:

Deleting a Manual Backup
========================

Function
--------

This API is used to delete a manual backup. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

   DELETE https://{*Endpoint*}/v3/{project_id}/backups/{backup_id}

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/backups/e28d08754b1a490fb2b3540ed013a7fbbr14

-  Parameter description

   .. table:: **Table 1** Parameter description

      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                   |
      +=================+=================+=================+===============================================================================+
      | project_id      | Yes             | String          | Project ID of a tenant in a region.                                           |
      |                 |                 |                 |                                                                               |
      |                 |                 |                 | To obtain this value, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
      | backup_id       | Yes             | String          | Backup ID.                                                                    |
      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+

Request
-------

None

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

   202

-  Abnormal

   For details, see :ref:`Status Codes <opengauss_api_0032>`.

Error Code
----------

For details, see :ref:`Error Codes <opengauss_api_0033>`.
