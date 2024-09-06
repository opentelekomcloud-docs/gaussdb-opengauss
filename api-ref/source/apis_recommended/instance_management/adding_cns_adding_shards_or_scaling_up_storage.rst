:original_name: opengauss_api_0016.html

.. _opengauss_api_0016:

Adding CNs, Adding Shards, or Scaling Up Storage
================================================

Function
--------

This API is used to add CNs, add shards, or scale up storage. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

Constraints
-----------

-  Adding CNs

   -  The maximum number of CNs is 256.
   -  If you choose the single-AZ deployment during instance creation, add CNs in the same AZ.

-  Adding shards

   -  The shard growth increment ranges from 1 to 64.
   -  The maximum number of shards is 256.

-  Scaling up Storage

   -  The storage space must be a multiple of (Number of shards x 40 GB).
   -  All nodes must be available.

URI
---

-  URI format

   POST https://{*Endpoint*}/v3/{project_id}/instances/{instance_id}/action

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in14/action

-  Parameter description

   .. table:: **Table 1** Parameter description

      +-----------------------+-----------------------+-------------------------------------------------------------------------------+
      | Name                  | Mandatory             | Description                                                                   |
      +=======================+=======================+===============================================================================+
      | project_id            | Yes                   | Project ID of a tenant in a region.                                           |
      |                       |                       |                                                                               |
      |                       |                       | To obtain this value, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------+
      | instance_id           | Yes                   | Instance ID.                                                                  |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------+

Request
-------

-  Parameter description

   .. table:: **Table 2** Parameter description

      +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------+
      | Name            | Mandatory       | Type            | Description                                                                             |
      +=================+=================+=================+=========================================================================================+
      | expand_cluster  | No              | Object          | This parameter is mandatory when you add CNs or shards.                                 |
      |                 |                 |                 |                                                                                         |
      |                 |                 |                 | For details, see :ref:`Table 3 <en-us_topic_0000001917290492__table444432914102>`.      |
      +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------+
      | enlarge_volume  | No              | Object          | New storage space after scaling up. This parameter is mandatory for scaling up storage. |
      |                 |                 |                 |                                                                                         |
      |                 |                 |                 | For details, see :ref:`Table 6 <en-us_topic_0000001917290492__table11761269>`.          |
      +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001917290492__table444432914102:

   .. table:: **Table 3** expand_cluster field data structure description

      +--------------+-----------+-------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
      | Name         | Mandatory | Type                          | Description                                                                                                                        |
      +==============+===========+===============================+====================================================================================================================================+
      | coordinators | No        | Array of Coordinators objects | This parameter is mandatory for adding CNs. For details, see :ref:`Table 4 <en-us_topic_0000001917290492__table246654931220>`.     |
      +--------------+-----------+-------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
      | shard        | No        | Shard object                  | This parameter is mandatory for adding shards. For details, see :ref:`Table 5 <en-us_topic_0000001917290492__table5142851121216>`. |
      +--------------+-----------+-------------------------------+------------------------------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001917290492__table246654931220:

   .. table:: **Table 4** coordinators parameter description

      +---------+-----------+--------+------------------------------------------------------------------------------------------------------------+
      | Name    | Mandatory | Type   | Description                                                                                                |
      +=========+===========+========+============================================================================================================+
      | az_code | Yes       | string | AZs to which CNs are to be added. If multiple CNs need to be added, enter the AZ where each CN is located. |
      +---------+-----------+--------+------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001917290492__table5142851121216:

   .. table:: **Table 5** shard parameter description

      ===== ========= ======= =============================
      Name  Mandatory Type    Description
      ===== ========= ======= =============================
      count Yes       integer Number of shards to be added.
      ===== ========= ======= =============================

   .. _en-us_topic_0000001917290492__table11761269:

   .. table:: **Table 6** enlarge_volume field data structure description

      +------+-----------+---------+------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name | Mandatory | Type    | Description                                                                                                                                          |
      +======+===========+=========+======================================================================================================================================================+
      | size | Yes       | Integer | Storage space, which must always be a multiple of (Number of shards x 40 GB). Value range: (Number of shards x 40 GB) to (Number of shards x 16 TB). |
      +------+-----------+---------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

-  Adding a CN

   .. code-block::

      {
          "expand_cluster": {
              "coordinators": [
                  {
                      "az_code": "eu-de-01"
                  }
              ]
          }
      }

-  Adding multiple CNs

   .. code-block::

      {
          "expand_cluster": {
              "coordinators": [
                  {
                      "az_code": "eu-de-01"
                  },
                  {
                      "az_code": "eu-de-01"
                  },
                  {
                      "az_code": "eu-de-01"
                  }
              ]
          }
      }

-  Adding a DN shard

   .. code-block::

      {
          "expand_cluster": {
              "shard": {
                  "count": 1
              }
          }
      }

-  Scaling up storage to 400 GB

   .. code-block::

      {
          "enlarge_volume": {
              "size": 400
          }
      }

Response
--------

-  Normal response

   .. table:: **Table 7** Parameter description

      +--------+--------+------------------------------------------------------------------------------------------+
      | Name   | Type   | Description                                                                              |
      +========+========+==========================================================================================+
      | job_id | String | Task ID. This parameter is returned when your instance is billed at a pay-per-use basis. |
      +--------+--------+------------------------------------------------------------------------------------------+

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
