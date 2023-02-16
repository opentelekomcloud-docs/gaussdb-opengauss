:original_name: opengauss_api_0024.html

.. _opengauss_api_0024:

Modifying Parameters of a Specified DB Instance
===============================================

Function
--------

This API is used to modify parameters in the parameter template of a specified DB instance.

-  Before calling an API, you need to understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Before calling this API, obtain the required `region and endpoint <https://docs.otc.t-systems.com/en-us/endpoint/index.html>`__.

Constraints
-----------

-  The values of the edited parameters must be within the default value range of the specified database version. For details about the range of parameter values, see "Modifying Parameters in a Parameter Template" in the *GaussDB(openGauss) User Guide*.

URI
---

-  URI format

   PUT https://{*Endpoint*}/opengauss/v3/{project_id}/instances/{instance_id}/configurations

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/opengauss/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in01/configurations

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

      +--------+-----------+--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name   | Mandatory | Type               | Description                                                                                                                                                                        |
      +========+===========+====================+====================================================================================================================================================================================+
      | values | Yes       | Map<String,String> | Specifies the parameter values defined by users based on the default parameter templates. For details, see :ref:`Table 3 <opengauss_api_0024__t4d55d375227f4efd85a5d345d4eddd2c>`. |
      +--------+-----------+--------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. _opengauss_api_0024__t4d55d375227f4efd85a5d345d4eddd2c:

   .. table:: **Table 3** values field data structure description

      +-------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Name  | Mandatory | Type   | Description                                                                                                                          |
      +=======+===========+========+======================================================================================================================================+
      | key   | Yes       | String | Specifies the parameter name. For example, for the **"failed_login_attempts": "4"** parameter, the key is **failed_login_attempts**. |
      +-------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------+
      | value | Yes       | String | Specifies the parameter value. For example, for the **"failed_login_attempts": "4"** parameter, the value is **4**.                  |
      +-------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------+

-  Request example

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
      | restart_required      | Boolean               | Indicates whether a reboot is required. |
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

Status Codes
------------

For details, see :ref:`Status Codes <opengauss_api_0032>`.

Error Codes
-----------

For details, see :ref:`Error Codes <opengauss_api_0033>`.
