:original_name: opengauss_newapi_0004.html

.. _opengauss_newapi_0004:

Obtaining the Parameters of a Specified DB Instance
===================================================

Function
--------

This API is used to obtain parameters of a specified DB instance. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

   GET https://{*Endpoint*}/v3/{project_id}/instances/{instance_id}/configurations

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in14/configurations

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

      +--------------------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                | Type             | Description                                                                                                                                                            |
      +==========================+==================+========================================================================================================================================================================+
      | datastore_version        | String           | Engine version.                                                                                                                                                        |
      +--------------------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | datastore_name           | String           | Engine name.                                                                                                                                                           |
      +--------------------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | created                  | String           | Creation time in the "yyyy-MM-dd HH:mm:ss" format.                                                                                                                     |
      +--------------------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | updated                  | String           | Update time in the "yyyy-MM-ddHH:mm:ss" format.                                                                                                                        |
      +--------------------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | configuration_parameters | Array of objects | Parameters defined by users based on the default parameter templates. For details, see :ref:`Table 3 <en-us_topic_0000001947569493__response_configurationparameter>`. |
      +--------------------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001947569493__response_configurationparameter:

   .. table:: **Table 3** configuration_parameters field data structure description

      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------+
      | Parameter             | Type                  | Description                                                                                    |
      +=======================+=======================+================================================================================================+
      | name                  | String                | Parameter name.                                                                                |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------+
      | value                 | String                | Parameter value.                                                                               |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------+
      | restart_required      | Boolean               | Whether a reboot is required after the parameter is modified.                                  |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------+
      | value_range           | String                | Parameter value range.                                                                         |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------+
      | type                  | String                | Parameter type. The value can be **string**, **integer**, **boolean**, **list**, or **float**. |
      |                       |                       |                                                                                                |
      |                       |                       | Value:                                                                                         |
      |                       |                       |                                                                                                |
      |                       |                       | -  **string**                                                                                  |
      |                       |                       | -  **integer**                                                                                 |
      |                       |                       | -  **boolean**                                                                                 |
      |                       |                       | -  **list**                                                                                    |
      |                       |                       | -  **float**                                                                                   |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------+
      | description           | String                | Parameter description.                                                                         |
      +-----------------------+-----------------------+------------------------------------------------------------------------------------------------+

Example Response
----------------

-  Normal response

   .. code-block:: text

      {
          "created": "2022-04-11 10:46:59",
          "updated": "2022-04-11 10:46:59",
          "datastore_version": "2.0",
          "datastore_name": "GaussDB(for openGauss)",
          "configuration_parameters": [
              {
                  "name": "audit_system_object",
                  "value": "12295",
                  "type": "integer",
                  "description": "Whether to audit the CREATE, DROP, and ALTER operations on database objects",
                  "restart_required": false,
                  "value_range": "0-2097151"
              }
          ]
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
