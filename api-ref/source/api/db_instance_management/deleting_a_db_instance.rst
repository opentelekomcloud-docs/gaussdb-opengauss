:original_name: opengauss_api_0017.html

.. _opengauss_api_0017:

Deleting a DB Instance
======================

Function
--------

This API is used to delete a DB instance.

-  Before calling an API, you need to understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Before calling this API, obtain the required `region and endpoint <https://docs.otc.t-systems.com/en-us/endpoint/index.html>`__.

URI
---

-  URI format

   DELETE https://{*Endpoint*}/opengauss/v3/{project_id}/instances/{instance_id}

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/opengauss/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in01

-  Parameter description

   .. table:: **Table 1** Parameter description

      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+
      | Name                  | Mandatory             | Description                                                                                             |
      +=======================+=======================+=========================================================================================================+
      | project_id            | Yes                   | Specifies the project ID of a tenant in a region.                                                       |
      |                       |                       |                                                                                                         |
      |                       |                       | For details about how to obtain the project ID, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+
      | instance_id           | Yes                   | Specifies the DB instance ID compliant with the UUID format.                                            |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

-  Normal response

   .. table:: **Table 2** Parameter description

      ====== ====== ==================================================
      Name   Type   Description
      ====== ====== ==================================================
      job_id String Indicates the ID of the DB instance deletion task.
      ====== ====== ==================================================

-  Example normal response

   .. code-block:: text

      {
          "job_id": "dff1d289-4d03-4942-8b9f-463ea07c000d"
      }

-  Abnormal response

   For details, see :ref:`Abnormal Request Results <opengauss_api_0031>`.

Status Codes
------------

For details, see :ref:`Status Codes <opengauss_api_0032>`.

Error Codes
-----------

For details, see :ref:`Error Codes <opengauss_api_0033>`.
