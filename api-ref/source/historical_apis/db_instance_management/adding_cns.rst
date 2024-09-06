:original_name: opengauss_api_1019.html

.. _opengauss_api_1019:

Adding CNs
==========

Function
--------

This API is used to add CNs.

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

Constraints
-----------

-  The CN growth increment ranges from 1 to 9.
-  The maximum number of CNs is 256.
-  If you choose the single-AZ deployment during instance creation, add CNs in the same AZ.
-  The number of CNs of a DB instance cannot exceed twice the number of shards.

URI
---

-  URI format

   POST https://{*Endpoint*}/opengauss/v3/{*project_id*}/instances/{*instance_id*}/action

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

      +----------------+-----------+--------+----------------------------------------------------------------------------------------------------------+
      | Name           | Mandatory | Type   | Description                                                                                              |
      +================+===========+========+==========================================================================================================+
      | expand_cluster | Yes       | Object | For details, see :ref:`Table 3 <en-us_topic_0000001947569549__en-us_topic_0248254027_table68501959278>`. |
      +----------------+-----------+--------+----------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001947569549__en-us_topic_0248254027_table68501959278:

   .. table:: **Table 3** expand_cluster field data structure description

      +--------------+-----------+-------+-----------------------------------------------------------------------------------------------------------+
      | Name         | Mandatory | Type  | Description                                                                                               |
      +==============+===========+=======+===========================================================================================================+
      | coordinators | Yes       | Array | For details, see :ref:`Table 4 <en-us_topic_0000001947569549__en-us_topic_0248254027_table968519176105>`. |
      +--------------+-----------+-------+-----------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001947569549__en-us_topic_0248254027_table968519176105:

   .. table:: **Table 4** azCode field data structure description

      ======= ========= ====== =================================
      Name    Mandatory Type   Description
      ======= ========= ====== =================================
      az_code Yes       String AZs to which CNs are to be added.
      ======= ========= ====== =================================

-  Example request (adding a single CN)

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

-  Example request (adding multiple CNs)

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
