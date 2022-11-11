:original_name: opengauss_api_0019.html

.. _opengauss_api_0019:

Adding CNs
==========

Function
--------

This API is used to add CNs.

-  Before calling an API, you need to understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Before calling this API, obtain the required `region and endpoint <https://docs.otc.t-systems.com/en-us/endpoint/index.html>`__.

Constraints
-----------

-  The CN growth increment ranges from 1 to 9.
-  The maximum number of CNs is 256.
-  If you choose the single-AZ deployment during instance creation, add CNs in the same AZ.
-  The number of CNs for a DB instance must be less than or equal to twice the number of shards.

URI
---

-  URI format

   POST https://{*Endpoint*}/opengauss/v3/{*project_id*}/instances/{*instance_id*}/action

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/opengauss/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in01/action

-  Parameter description

   .. table:: **Table 1** Parameter description

      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+
      | Name                  | Mandatory             | Description                                                                                             |
      +=======================+=======================+=========================================================================================================+
      | project_id            | Yes                   | Specifies the project ID of a tenant in a region.                                                       |
      |                       |                       |                                                                                                         |
      |                       |                       | For details about how to obtain the project ID, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+
      | instance_id           | Yes                   | Specifies the DB instance ID.                                                                           |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+

Request
-------

-  Parameter description

   .. table:: **Table 2** Parameter description

      +----------------+-----------+--------+------------------------------------------------------------------------------------------+
      | Name           | Mandatory | Type   | Description                                                                              |
      +================+===========+========+==========================================================================================+
      | expand_cluster | Yes       | Object | For details, see :ref:`Table 3 <opengauss_api_0019__ta3716774d7224da5a2e283ca6b14a3f8>`. |
      +----------------+-----------+--------+------------------------------------------------------------------------------------------+

   .. _opengauss_api_0019__ta3716774d7224da5a2e283ca6b14a3f8:

   .. table:: **Table 3** expand_cluster field data structure description

      +--------------+-----------+-------+------------------------------------------------------------------------------------------+
      | Name         | Mandatory | Type  | Description                                                                              |
      +==============+===========+=======+==========================================================================================+
      | coordinators | Yes       | Array | For details, see :ref:`Table 4 <opengauss_api_0019__t5f8b19c731a94a8f8a4f9d12c997c911>`. |
      +--------------+-----------+-------+------------------------------------------------------------------------------------------+

   .. _opengauss_api_0019__t5f8b19c731a94a8f8a4f9d12c997c911:

   .. table:: **Table 4** azCode field data structure description

      ======= ========= ====== ==============================================
      Name    Mandatory Type   Description
      ======= ========= ====== ==============================================
      az_code Yes       String Specifies the AZ to which CNs are to be added.
      ======= ========= ====== ==============================================

-  Example request (adding CNs for a single-CN instance)

   .. code-block:: text

      {
          "expand_cluster": {
                      "coordinators": [
                              {
                                     "az_code":"eu-de-01"
                              }
                      ]
          }
      }

-  Example request (adding CNs for a multi-CN instance)

   .. code-block:: text

      {
          "expand_cluster": {
                      "coordinators": [
                              {
                                     "az_code":"eu-de-01"
                              },
                              {
                                     "az_code":"eu-de-01"
                              },
                              {
                                     "az_code":"eu-de-01"
                              }
                      ]
          }
      }

Response
--------

-  Normal response

   .. table:: **Table 5** Parameter description

      ====== ====== ======================
      Name   Type   Description
      ====== ====== ======================
      job_id String Specifies the task ID.
      ====== ====== ======================

-  Example normal response

   .. code-block:: text

      {
          "job_id": "2b414788a6004883a02390e2eb0ea227"
      }

-  Abnormal response

   For details, see :ref:`Abnormal Request Results <opengauss_api_0031>`.

Status Codes
------------

For details, see :ref:`Status Codes <opengauss_api_0032>`.

Error Codes
-----------

For details, see :ref:`Error Codes <opengauss_api_0033>`.
