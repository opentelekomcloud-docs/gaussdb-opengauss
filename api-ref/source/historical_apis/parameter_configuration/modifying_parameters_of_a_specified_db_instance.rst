:original_name: opengauss_api_1024.html

.. _opengauss_api_1024:

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

-  The values of the modified parameters must be within the default value range of the specified database version. For details about the value range of parameters, see "Modifying Parameters in a Parameter Template" in *GaussDB User Guide*.

URI
---

-  URI format

   PUT https://{*Endpoint*}/opengauss/v3/{project_id}/instances/{instance_id}/configurations

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/opengauss/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in01/configurations

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

      +--------+-----------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name   | Mandatory | Type               | Description                                                                                                                                                                         |
      +========+===========+====================+=====================================================================================================================================================================================+
      | values | Yes       | Map<String,String> | Parameter values defined by users based on a default parameter template For details, see :ref:`Table 3 <en-us_topic_0000001917290728__en-us_topic_0136489341_table12745180163820>`. |
      +--------+-----------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001917290728__en-us_topic_0136489341_table12745180163820:

   .. table:: **Table 3** values field data structure description

      +-------+-----------+--------+------------------------------------------------------------------------------------------------------------------------+
      | Name  | Mandatory | Type   | Description                                                                                                            |
      +=======+===========+========+========================================================================================================================+
      | key   | Yes       | String | Parameter name. For example, for the **"failed_login_attempts": "4"** parameter, the key is **failed_login_attempts**. |
      +-------+-----------+--------+------------------------------------------------------------------------------------------------------------------------+
      | value | Yes       | String | Parameter value. For example, for the **"failed_login_attempts": "4"** parameter, the value is **4**.                  |
      +-------+-----------+--------+------------------------------------------------------------------------------------------------------------------------+

-  Example request

   .. code-block:: text

      {
          "values": {
             "xxx": "10",
             "yyy": "OFF"
          }
      }

Response
--------

-  Normal response

   .. table:: **Table 4** Parameter description

      +-----------------------+-----------------------+-----------------------------------------+
      | Name                  | Type                  | Description                             |
      +=======================+=======================+=========================================+
      | restart_required      | Boolean               | Whether a reboot is required.           |
      |                       |                       |                                         |
      |                       |                       | -  **true**: A reboot is required.      |
      |                       |                       | -  **false**: A reboot is not required. |
      +-----------------------+-----------------------+-----------------------------------------+

-  Example normal response

   .. code-block:: text

      {
        "restart_required": false
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
