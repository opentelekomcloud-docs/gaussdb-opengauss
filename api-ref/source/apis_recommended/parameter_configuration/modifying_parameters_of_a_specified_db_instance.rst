:original_name: opengauss_api_0024.html

.. _opengauss_api_0024:

Modifying Parameters of a Specified DB Instance
===============================================

Function
--------

This API is used to modify parameters in the parameter template of a specified DB instance. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

Constraints
-----------

The values of the modified parameters must be within the default value range of the specified database version.

URI
---

-  URI format

   PUT https://{*Endpoint*}/v3/{project_id}/instances/{instance_id}/configurations

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in01/configurations

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

      +-----------------+-----------------+--------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name            | Mandatory       | Type               | Description                                                                                                                                         |
      +=================+=================+====================+=====================================================================================================================================================+
      | values          | Yes             | Map<String,String> | Parameter values defined by users based on the default parameter templates.                                                                         |
      |                 |                 |                    |                                                                                                                                                     |
      |                 |                 |                    | Example: For **failed_login_attempts: 4**, **failed_login_attempts** indicates the parameter name, and **4** indicated the changed parameter value. |
      +-----------------+-----------------+--------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

-  Changing the value of **failed_login_attempts** to **4** (The change is applied without a DB instance reboot.)

   .. code-block::

      {
          "values": {
              "failed_login_attempts": "4"
          }
      }

-  Changing the value of **track_activity_query_size** to **2048** and the value of **max_replication_slots** to **25** (The changes are applied after the DB instance is rebooted.)

   .. code-block::

      {
          "values": {
              "track_activity_query_size": "2048",
              "max_replication_slots": "30"
          }
      }

Response
--------

-  Normal response

   .. table:: **Table 3** Parameter description

      +-----------------------+-----------------------+-----------------------------------------+
      | Name                  | Type                  | Description                             |
      +=======================+=======================+=========================================+
      | restart_required      | Boolean               | Whether a reboot is required.           |
      |                       |                       |                                         |
      |                       |                       | -  **true**: A reboot is required.      |
      |                       |                       | -  **false**: A reboot is not required. |
      +-----------------------+-----------------------+-----------------------------------------+

-  Example normal response

   -  A reboot is not required.

      .. code-block:: text

         {
           "restart_required": false
         }

   -  A reboot is required.

      .. code-block:: text

         {
           "restart_required": true
         }

-  Abnormal response

   For details, see :ref:`Abnormal Request Results <opengauss_api_0031>`.

Status Code
-----------

-  Normal

   200

-  Abnormal

   For details, see :ref:`Status Codes <opengauss_api_0032>`.

Error Code
----------

For details, see :ref:`Error Codes <opengauss_api_0033>`.
