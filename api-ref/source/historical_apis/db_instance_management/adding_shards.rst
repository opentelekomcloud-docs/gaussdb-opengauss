:original_name: opengauss_api_1020.html

.. _opengauss_api_1020:

Adding Shards
=============

Function
--------

This API is used to add shards.

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

.. note::

   Intermittent disconnection occurs when shards are being added. Exercise caution when performing this operation.

Constraints
-----------

-  The shard growth increment ranges from 1 to 64.
-  The maximum number of shards is 256.

URI
---

-  URI format

   POST https://{*Endpoint*}/opengauss/v3/{*project_id*}/instances/{*instance_id*}/action

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/opengauss/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in01/action

   -  Parameter description

      .. table:: **Table 1** Parameter description

         +----------------+-----------+--------+-------------------------------------------------------------------------------------------------------------+
         | Name           | Mandatory | Type   | Description                                                                                                 |
         +================+===========+========+=============================================================================================================+
         | expand_cluster | Yes       | Object | For details, see :ref:`Table 2 <en-us_topic_0000001947569473__en-us_topic_0248254028_table15864348201411>`. |
         +----------------+-----------+--------+-------------------------------------------------------------------------------------------------------------+

      .. _en-us_topic_0000001947569473__en-us_topic_0248254028_table15864348201411:

      .. table:: **Table 2** expand_cluster field data structure description

         +-------+-----------+--------+-----------------------------------------------------------------------------------------------------------+
         | Name  | Mandatory | Type   | Description                                                                                               |
         +=======+===========+========+===========================================================================================================+
         | shard | Yes       | Object | For details, see :ref:`Table 3 <en-us_topic_0000001947569473__en-us_topic_0248254028_table968519176105>`. |
         +-------+-----------+--------+-----------------------------------------------------------------------------------------------------------+

      .. _en-us_topic_0000001947569473__en-us_topic_0248254028_table968519176105:

      .. table:: **Table 3** count field data structure description

         ===== ========= ======= =============================
         Name  Mandatory Type    Description
         ===== ========= ======= =============================
         count Yes       Integer Number of shards to be added.
         ===== ========= ======= =============================

-  Example request

   .. code-block:: text

      {
          "expand_cluster": {
                      "shard":{
                             "count":1
                      }

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

   -  Example response

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
