:original_name: opengauss_api_1016.html

.. _opengauss_api_1016:

Scaling up Storage Space of a DB Instance
=========================================

Function
--------

This API is used to scale up storage space of a DB instance. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

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

      +-----------------------+-----------------------+-------------------------------------------------------------------------------+
      | Name                  | Mandatory             | Description                                                                   |
      +=======================+=======================+===============================================================================+
      | project_id            | Yes                   | Project ID of a tenant in a region.                                           |
      |                       |                       |                                                                               |
      |                       |                       | To obtain this value, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------+
      | instance_id           | Yes                   | DB instance ID.                                                               |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------+

Request
-------

-  Parameter description

   .. table:: **Table 2** Parameter description

      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
      | Name            | Mandatory       | Type            | Description                                                                                                 |
      +=================+=================+=================+=============================================================================================================+
      | enlarge_volume  | Yes             | Object          | Target storage space after scaling up.                                                                      |
      |                 |                 |                 |                                                                                                             |
      |                 |                 |                 | For details, see :ref:`Table 3 <en-us_topic_0000001917130496__en-us_topic_0151958051_table17521151123114>`. |
      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001917130496__en-us_topic_0151958051_table17521151123114:

   .. table:: **Table 3** enlarge_volume field data structure description

      +------+-----------+---------+------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name | Mandatory | Type    | Description                                                                                                                                          |
      +======+===========+=========+======================================================================================================================================================+
      | size | Yes       | Integer | Storage space, which must always be a multiple of (Number of shards x 40 GB). Value range: (Number of shards x 40 GB) to (Number of shards x 16 TB). |
      +------+-----------+---------+------------------------------------------------------------------------------------------------------------------------------------------------------+

-  Example request

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

      ====== ====== ===========
      Name   Type   Description
      ====== ====== ===========
      job_id String Task ID.
      ====== ====== ===========

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
