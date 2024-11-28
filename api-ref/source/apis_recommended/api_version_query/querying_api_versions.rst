:original_name: opengauss_query_version_list.html

.. _opengauss_query_version_list:

Querying API Versions
=====================

Function
--------

This API is used to query the supported API versions. Before calling this API:

-  Learn how to :ref:`authenticate <opengauss_api_0010>` this API.
-  Understand the API in :ref:`Using APIs <opengauss_api_0012>`.
-  Obtain the required `region and endpoint <https://docs.otc.t-systems.com/en-us/endpoint/index.html>`__.

URI
---

-  URI format

   GET https://{*Endpoint*}

-  Example

   https://gaussdb.eu-de.otc.t-systems.com

Request
-------

None

Response
--------

-  Normal response

   .. table:: **Table 1** Parameter description

      +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------+
      | Name                  | Type                  | Description                                                                                               |
      +=======================+=======================+===========================================================================================================+
      | versions              | Array of objects      | API version information.                                                                                  |
      |                       |                       |                                                                                                           |
      |                       |                       | For details, see :ref:`Table 2 <en-us_topic_0000001917290504__en-us_topic_0128427215_table578797132615>`. |
      +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001917290504__en-us_topic_0128427215_table578797132615:

   .. table:: **Table 2** versions field data structure description

      +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------+
      | Name                  | Type                  | Description                                                                                                 |
      +=======================+=======================+=============================================================================================================+
      | id                    | String                | Backup time window. The creation of an automated backup will be triggered during the backup time window.    |
      |                       |                       |                                                                                                             |
      |                       |                       | The time is in the UTC format.                                                                              |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------+
      | links                 | Array of objects      | API link information.                                                                                       |
      |                       |                       |                                                                                                             |
      |                       |                       | For details, see :ref:`Table 3 <en-us_topic_0000001917290504__en-us_topic_0128427215_table14771167122611>`. |
      |                       |                       |                                                                                                             |
      |                       |                       | .. note::                                                                                                   |
      |                       |                       |                                                                                                             |
      |                       |                       |    If the version is v3 adn v 3.1, the value is **[]**.                                                     |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------+
      | status                | String                | Version status.                                                                                             |
      |                       |                       |                                                                                                             |
      |                       |                       | The value is **CURRENT**, indicating that the version has been released.                                    |
      |                       |                       |                                                                                                             |
      |                       |                       | **SUPPORTED**: The version is an earlier version, but it is still supported.                                |
      |                       |                       |                                                                                                             |
      |                       |                       | **DEPRECATED**: The version is a deprecated version, which may be deleted later.                            |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------+
      | version               | String                | Subversion information of the API version.                                                                  |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------+
      | min_version           | String                | Minimum API version number.                                                                                 |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------+
      | updated               | String                | Version update time.                                                                                        |
      |                       |                       |                                                                                                             |
      |                       |                       | The format is yyyy-mm-ddThh:mm:ssZ.                                                                         |
      |                       |                       |                                                                                                             |
      |                       |                       | **T** is the separator between the calendar and the hourly notation of time. **Z** indicates the UTC.       |
      +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001917290504__en-us_topic_0128427215_table14771167122611:

   .. table:: **Table 3** links field data structure description

      ==== ====== ===========================================================
      Name Type   Description
      ==== ====== ===========================================================
      href String API URL. The value is **""**.
      rel  String The value is **self**, indicating that URL is a local link.
      ==== ====== ===========================================================

-  Example normal response

   .. code-block:: text

      {
          "versions": [
              {
                  "id": "v3",
                  "links": [],
                  "status": "CURRENT",
                  "version": "",
                  "min_version": "",
                  "updated": "2017-02-07T17:34:02Z"
              },
              {
                  "id": "v3.1",
                  "links": [],
                  "status": "CURRENT",
                  "version": "",
                  "min_version": "",
                  "updated": "2017-02-07T17:34:02Z"
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
