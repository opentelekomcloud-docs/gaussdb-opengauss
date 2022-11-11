:original_name: opengauss_api_0022.html

.. _opengauss_api_0022:

Modifying a DB Instance Name
============================

Function
--------

This API is used to change a DB instance name.

-  Before calling an API, you need to understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Before calling this API, obtain the required `region and endpoint <https://docs.otc.t-systems.com/en-us/endpoint/index.html>`__.

URI
---

-  URI format

   PUT https://{*Endpoint*}/opengauss/v3/{*project_id*}/instances/{*instance_id*}/name

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/opengauss/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in14/name

   -  Parameter description

      .. table:: **Table 1** Parameter description

         +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+
         | Name                  | Mandatory             | Description                                                                                             |
         +=======================+=======================+=========================================================================================================+
         | project_id            | Yes                   | Specifies the project ID of a tenant in a region.                                                       |
         |                       |                       |                                                                                                         |
         |                       |                       | For details about how to obtain the project ID, see :ref:`Obtaining a Project ID <opengauss_api_0034>`. |
         +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+
         | instance_id           | Yes                   | Specifies the DB instance ID, which is compliant with the UUID format.                                  |
         +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------+

Request
-------

-  Parameter description

   .. table:: **Table 2** Parameter description

      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name            | Mandatory       | Type            | Description                                                                                                                                                  |
      +=================+=================+=================+==============================================================================================================================================================+
      | name            | Yes             | String          | Specifies the DB instance name. DB instances of the same type can have same names under the same tenant.                                                     |
      |                 |                 |                 |                                                                                                                                                              |
      |                 |                 |                 | The value consists of 4 to 64 characters and starts with a letter. It is case-sensitive and contains only letters, digits, hyphens (-), and underscores (_). |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  Example request

   .. code-block:: text

      {
        "name": "instance-name"
      }

Response
--------

-  Normal responses

   .. table:: **Table 3** Parameter description

      +--------+--------+---------------------------------------------------------------+
      | Name   | Type   | Description                                                   |
      +========+========+===============================================================+
      | job_id | String | Indicates the ID of the task for changing a DB instance name. |
      +--------+--------+---------------------------------------------------------------+

-  Example normal response

   .. code-block:: text

      {
          "job_id": "2b414788a6004883a02390e2eb0ea227"
      }

-  Abnormal response

   For details, see :ref:`Abnormal Request Results <opengauss_api_0031>`.

Status Codes
------------

For details, see :ref:`Status Codes <opengauss_api_0032>`.

Error Codes
-----------

For details, see :ref:`Error Codes <opengauss_api_0033>`.
