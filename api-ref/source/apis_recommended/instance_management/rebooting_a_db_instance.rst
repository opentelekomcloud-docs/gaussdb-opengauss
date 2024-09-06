:original_name: opengauss_newapi_0001.html

.. _opengauss_newapi_0001:

Rebooting a DB Instance
=======================

Function
--------

This API is used to reboot a DB instance. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

Constraints
-----------

The instance cannot reboot when it is being created, scaled, backed up, and restored.

URI
---

-  URI format

   POST https://{*Endpoint*}/v3/{project_id}/instances/{instance_id}/restart

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in14/restart

-  Parameter description

   .. table:: **Table 1** Parameter description

      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                   |
      +=================+=================+=================+===============================================================================+
      | project_id      | Yes             | String          | Project ID of a tenant in a region.                                           |
      |                 |                 |                 |                                                                               |
      |                 |                 |                 | To obtain this value, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
      | instance_id     | Yes             | String          | DB instance ID.                                                               |
      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+

Request
-------

None

Response
--------

-  Normal response

   .. table:: **Table 2** Parameter description

      ========= ====== ===========
      Parameter Type   Description
      ========= ====== ===========
      job_id    String Task ID.
      ========= ====== ===========

-  Example normal response

   .. code-block:: text

      {
          "job_id": "2b414788a6004883a02390e2eb0ea227"
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
