:original_name: opengauss_api_0021.html

.. _opengauss_api_0021:

Resetting a Database Password
=============================

Function
--------

This API is used to reset a database password.

-  Before calling an API, you need to understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Before calling this API, obtain the required `region and endpoint <https://docs.otc.t-systems.com/en-us/endpoint/index.html>`__.

URI
---

-  URI format

   POST https://{*Endpoint*}/opengauss/v3/{*project_id*}/instances/{*instance_id*}/password

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/opengauss/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in01/password

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

      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name            | Mandatory       | Type            | Description                                                                                                                                                                                          |
      +=================+=================+=================+======================================================================================================================================================================================================+
      | password        | Yes             | String          | Specifies the password for user **root**.                                                                                                                                                            |
      |                 |                 |                 |                                                                                                                                                                                                      |
      |                 |                 |                 | -  It consists of 8 to 32 characters and contains at least three types of the following characters: uppercase letters, lowercase letters, digits, and special characters (``~!``\ ``@#$%^*-_=+?,``). |
      |                 |                 |                 | -  It supports weak password verification.                                                                                                                                                           |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  Example request

   .. code-block:: text

      {
        "password": "Test_345612"
      }

Response
--------

-  Normal responses

   None

-  Example normal response

   {}

-  Abnormal response

   For details, see :ref:`Abnormal Request Results <opengauss_api_0031>`.

Status Codes
------------

For details, see :ref:`Status Codes <opengauss_api_0032>`.

Error Codes
-----------

For details, see :ref:`Error Codes <opengauss_api_0033>`.
