:original_name: opengauss_api_0016.html

.. _opengauss_api_0016:

Scaling Up Storage Space of a DB Instance
=========================================

Function
--------

This API is used to scale up storage space of a DB instance.

-  Before calling an API, you need to understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Before calling this API, obtain the required `region and endpoint <https://docs.otc.t-systems.com/en-us/endpoint/index.html>`__.

Constraints
-----------

-  The storage space must be a multiple of the number of shards multiplied by 40 GB.
-  All nodes must be available.

URI
---

-  URI format

   POST https://{*Endpoint*}/opengauss/v3/{project_id}/instances/{instance_id}/action

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

      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------+
      | Name            | Mandatory       | Type            | Description                                                                              |
      +=================+=================+=================+==========================================================================================+
      | enlarge_volume  | Yes             | Object          | Specifies the target storage space after scaling up.                                     |
      |                 |                 |                 |                                                                                          |
      |                 |                 |                 | For details, see :ref:`Table 3 <opengauss_api_0016__tb1bbcd595e9c42c58c9e8299b02d08d6>`. |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------+

   .. _opengauss_api_0016__tb1bbcd595e9c42c58c9e8299b02d08d6:

   .. table:: **Table 3** enlarge_volume field data structure description

      +------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name | Mandatory | Type    | Description                                                                                                                                                                                               |
      +======+===========+=========+===========================================================================================================================================================================================================+
      | size | Yes       | Integer | Specifies the storage space, which must always be an integral multiple of (Number of shards x 40 GB). Minimum storage space = Number of shards x 40 GB. Maximum storage space = Number of shards x 16 TB. |
      +------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  Request example

   .. code-block:: text

      {
          "enlarge_volume": {
              "size": 400
          }
      }

Response
--------

-  Normal response

   .. table:: **Table 4** Parameter description

      ====== ====== ======================
      Name   Type   Description
      ====== ====== ======================
      job_id String Indicates the task ID.
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
