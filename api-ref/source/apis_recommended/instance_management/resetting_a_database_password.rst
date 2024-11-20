:original_name: opengauss_api_0021.html

.. _opengauss_api_0021:

Resetting a Database Password
=============================

Function
--------

This API is used to reset a database password. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

   POST https://{*Endpoint*}/v3/{*project_id*}/instances/{*instance_id*}/password

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in01/password

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

      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------+
      | Name            | Mandatory       | Type            | Description                                                                                 |
      +=================+=================+=================+=============================================================================================+
      | password        | Yes             | String          | Password for user **root**. The password must:                                              |
      |                 |                 |                 |                                                                                             |
      |                 |                 |                 | -  Consist of 8 to 32 characters.                                                           |
      |                 |                 |                 |                                                                                             |
      |                 |                 |                 | -  Contain at least three types of the following characters:                                |
      |                 |                 |                 |                                                                                             |
      |                 |                 |                 |    Uppercase letters, lowercase letters, digits, and special characters ``~! @# %^*-_=+?,`` |
      |                 |                 |                 |                                                                                             |
      |                 |                 |                 | -  Support weak password verification.                                                      |
      +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------+

Example Request
---------------

Changing the password of user **root**

.. code-block::

   {
     "password": "******"
   }

Response
--------

-  Normal response

   None

-  Example normal response

   {}

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
