:original_name: opengauss_api_0022.html

.. _opengauss_api_0022:

Changing a DB Instance Name
===========================

Function
--------

This API is used to change an instance name. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

URI
---

-  URI format

   PUT https://{*Endpoint*}/v3/{*project_id*}/instances/{*instance_id*}/name

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in14/name

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

      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name            | Mandatory       | Type            | Description                                                                                                                                                  |
      +=================+=================+=================+==============================================================================================================================================================+
      | name            | Yes             | String          | DB instance name. Instances of the same type can have same names under the same tenant.                                                                      |
      |                 |                 |                 |                                                                                                                                                              |
      |                 |                 |                 | The name must consist of 4 to 64 characters and start with a letter. It can contain only letters (case-sensitive), digits, hyphens (-), and underscores (_). |
      +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Changing a DB instance name

.. code-block::

   {
     "name": "instance-name"
   }

Response
--------

-  Normal response

   .. table:: **Table 3** Parameter description

      ====== ====== ======================================
      Name   Type   Description
      ====== ====== ======================================
      job_id String Task ID for changing an instance name.
      ====== ====== ======================================

-  Example normal response

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
