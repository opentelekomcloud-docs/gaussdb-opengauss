:original_name: opengauss_api_1037.html

.. _opengauss_api_1037:

Changing the Port Number of a Specified DB Instance
===================================================

Function
--------

This API is used to change the port number of a specified DB instance. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

PUT https://{*Endpoint*}/v3/{project_id}/instances/{instance_id}/port

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/instances/cc6fd964d93f4003851dfc29d57d30a5in14/port

-  Parameter description

   .. table:: **Table 1** Path parameters

      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                   |
      +=================+=================+=================+===============================================================================+
      | project_id      | Yes             | String          | Project ID of a tenant in a region.                                           |
      |                 |                 |                 |                                                                               |
      |                 |                 |                 | To obtain this value, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
      | instance_id     | Yes             | String          | Instance ID, which is compliant with the UUID format.                         |
      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+

Request
-------

-  Parameter description

   .. table:: **Table 2** Request body parameters

      +------+-----------+---------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name | Mandatory | Type    | Description                                                                                                                                                                                                                                                      |
      +======+===========+=========+==================================================================================================================================================================================================================================================================+
      | port | No        | Integer | Instance port. Value range: **1024** to **39998** (excluding the following which are occupied by the system and cannot be used: 2378, 2379, 2380, 4999, 5000, 5999, 6000, 60001, 8097, 8098, 12016, 12017, 20049, 20050, 21731, 21732, 32122, 32123, and 32124). |
      +------+-----------+---------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  Example request

   .. code-block::

      {
         "port" : 8001
       }

Response
--------

-  Normal response

   .. table:: **Table 3** Response body parameters

      ========= ====== ======================================
      Parameter Type   Description
      ========= ====== ======================================
      job_id    String ID of the task for modifying the port.
      ========= ====== ======================================

-  Example response

   .. code-block::

      {
        "job_id" : "5cbb8a90-2253-4cff-8a13-49aa8f31dfb5"
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
