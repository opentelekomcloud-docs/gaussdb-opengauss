:original_name: opengauss_newapi_0029.html

.. _opengauss_newapi_0029:

Switching Roles of the Primary and Standby DNs in Shards
========================================================

Function
--------

This API is used to perform a primary/standby DN switchover for one or more shards. In a shard, only one standby node can be promoted to primary. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

   POST https://{*Endpoint*}/v3/{project_id}/instances/{instance_id}/switch-shard

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0c8243400d37468bb4aed3cc94c2911d/instances/f9b5f9b296ec6808e067in14/switch-shard

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

      +--------+-----------+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name   | Mandatory | Type  | Description                                                                                                                                                                                                                                                         |
      +========+===========+=======+=====================================================================================================================================================================================================================================================================+
      | shards | Yes       | Array | Nodes. You can switch standby DNs of multiple shards to primary DNs. The node information is the node IDs and component IDs of shards whose standby DNs are promoted to primary. For details, see :ref:`Table 3 <en-us_topic_0000001917130500__table989242214713>`. |
      +--------+-----------+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001917130500__table989242214713:

   .. table:: **Table 3** shards parameter description

      +--------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name         | Mandatory | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
      +==============+===========+========+=================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
      | node_id      | Yes       | String | ID of the node where the standby DN to be promoted to primary is deployed.                                                                                                                                                                                                                                                                                                                                                                                      |
      +--------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | component_id | Yes       | String | ID of the standby DN to be promoted to primary. It contains up to 7 characters. It cannot be null, an empty string or spaces. Before verifying and using it, spaces are automatically filtered out. The value contains at least three types of the following: uppercase letters, lowercase letters, digits, and underscores (_). For details about how to obtain the component ID, see :ref:`Querying the Components of a DB Instance <opengauss_newapi_0030>`. |
      +--------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Switching roles of primary and standby DNs in multiple shards

.. code-block::

   {
   "shards":[
                  {
                         "node_id":"0bc478b4d132494a8f7b804da521b4b2no14",
                         "component_id":"dn_6001"
                  },
                  {
                         "node_id":"53dee94c50574d36a0060db0a6b644f6no14",
                         "component_id":"dn_6004"
                  }
           ]
   }

Response
--------

-  Normal response

   .. table:: **Table 4** Parameter description

      +--------+--------+--------------------------------------------------------------------------+
      | Name   | Type   | Description                                                              |
      +========+========+==========================================================================+
      | job_id | String | ID of the task for switch standby DNs of multiple shards to primary DNs. |
      +--------+--------+--------------------------------------------------------------------------+

-  Example normal response

   .. code-block:: text

      {
          "job_id": "e96bbb23-e053-4bd0-b0b7-16ad3f5d9b6d"
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
