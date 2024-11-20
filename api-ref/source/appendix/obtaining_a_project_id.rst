:original_name: opengauss_api_0034.html

.. _opengauss_api_0034:

Obtaining a Project ID
======================

Scenarios
---------

When calling APIs, you need to specify the project ID in some URLs. To do so, you need to obtain the project ID using either of the following methods:

-  :ref:`Obtaining the Project ID by Calling an API <en-us_topic_0000001917130752__section85791974381>`
-  :ref:`Obtaining a Project ID from the Console <en-us_topic_0000001917130752__section1952915126435>`

.. _en-us_topic_0000001917130752__section85791974381:

Obtaining the Project ID by Calling an API
------------------------------------------

The API used to obtain a project ID is **GET https://**\ *{Endpoint}*\ **/v3/projects**. *{Endpoint}* is the IAM endpoint and can be obtained from `Regions and Endpoints <https://docs.otc.t-systems.com/regions-and-endpoints/index.html>`__. For details about API authentication, see :ref:`Authentication <opengauss_api_0010>`.

The following is an example response. **id** indicates the project ID.

.. code-block::

   {
       "projects": [
           {
               "domain_id": "65382450e8f64ac0870cd180d14e684b",
               "is_domain": false,
               "parent_id": "65382450e8f64ac0870cd180d14e684b",
               "name": "project_name",
               "description": "",
               "links": {
                   "next": null,
                   "previous": null,
                   "self": "https://www.example.com/v3/projects/a4a5d4098fb4474fa22cd05f897d6b99"
               },
               "id": "a4a5d4098fb4474fa22cd05f897d6b99",
               "enabled": true
           }
       ],
       "links": {
           "next": null,
           "previous": null,
           "self": "https://www.example.com/v3/projects"
       }
   }

.. _en-us_topic_0000001917130752__section1952915126435:

Obtaining a Project ID from the Console
---------------------------------------

#. Register yourself on the management console and log in to it.

#. On the **My Credential** page, view project IDs in the project list.


   .. figure:: /_static/images/en-us_image_0000001917290800.jpg
      :alt: **Figure 1** Viewing project IDs

      **Figure 1** Viewing project IDs
