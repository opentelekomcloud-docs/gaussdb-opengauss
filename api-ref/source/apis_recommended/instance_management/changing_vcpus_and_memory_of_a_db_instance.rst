:original_name: opengauss_newapi_0031.html

.. _opengauss_newapi_0031:

Changing vCPUs and Memory of a DB Instance
==========================================

Function
--------

This API is used to change the vCPUs and memory of a DB instance. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__.

Constraints
-----------

-  Currently, the instance specifications can only be scaled out. The new vCPUs and memory of an instance must be greater than the old vCPUs and memory.
-  The OS architecture of the new specifications must be the same as that of the old specifications.

URI
---

-  URI format

   PUT https://{*Endpoint*}/v3/{project_id}/instances/{instance_id}/flavor

-  Example

   https://gaussdb.eu-de.otc.t-systems.com/v3/0483b6b16e954cb88930a360d2c4e663/instances/dsfae23fsfdsae3435in14/flavor

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

      +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | Name       | Mandatory | Type   | Description                                                                                                                                       |
      +============+===========+========+===================================================================================================================================================+
      | flavor_ref | Yes       | String | New specification code. For details on how to obtain the specification code, see :ref:`Table 1 <en-us_topic_0000001917130736__table64111016147>`. |
      +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Changing the specifications of a pay-per-use DB instance from 4 vCPUs and 32 GB to 8 vCPUs and 64 GB

.. code-block::

   {
           "flavor_ref":"gaussdb.opengauss.ee.dn.m4.2xlarge.8.in"
    }

Response
--------

-  Normal response

   .. table:: **Table 3** Parameter description

      +--------+--------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | Name   | Type   | Description                                                                                                                                |
      +========+========+============================================================================================================================================+
      | job_id | String | Job ID for changing instance specifications. This parameter is returned only when you change the specifications of a pay-per-use instance. |
      +--------+--------+--------------------------------------------------------------------------------------------------------------------------------------------+

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
