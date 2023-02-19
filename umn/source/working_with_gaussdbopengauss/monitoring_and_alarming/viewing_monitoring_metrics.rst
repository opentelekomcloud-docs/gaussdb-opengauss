:original_name: opengauss_01_0073.html

.. _opengauss_01_0073:

Viewing Monitoring Metrics
==========================

Scenarios
---------

The Cloud Eye service monitors operating statuses of DB instances. You can view the GaussDB(openGauss) monitoring metrics on the management console. For details, see :ref:`Viewing Metrics of DB Instances <opengauss_01_0073__s674e6039596a4fe2b740de3d7eb94332>`.

Monitored data takes some time for transmission and display. The DB instance status displayed on the Cloud Eye console is the status of the last 5 to 10 minutes. If your DB instance is newly created, wait for 5 to 10 minutes and then view the monitoring data.

Prerequisites
-------------

-  A DB instance is running properly.

   Monitoring metrics of the DB instances that are faulty or have been deleted cannot be displayed on the Cloud Eye console. You can view their monitoring metrics after they are rebooted or restored to be normal.

.. note::

   If a DB instance has been faulty for 24 hours, Cloud Eye considers that it does not exist and deletes it from the monitoring object list. You need to manually clear the alarm rules created for the DB instance.

-  The DB instance keeps running properly for about 10 minutes.

   For a newly created DB instance, you need to wait for a while before viewing the monitoring metrics.

.. _opengauss_01_0073__s674e6039596a4fe2b740de3d7eb94332:

Viewing Metrics of DB Instances
-------------------------------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and a project.

#. Under **Management & Deployment**, click **Cloud Eye**.

#. In the navigation pane on the left, choose **Cloud Service Monitoring** > **GaussDB(openGauss)**.

#. On the Cloud Eye console, view monitoring metrics of DB instances.

   Cloud Eye supports monitoring of performance metrics in the last 1 hour, last 3 hours, or last 12 hours.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
