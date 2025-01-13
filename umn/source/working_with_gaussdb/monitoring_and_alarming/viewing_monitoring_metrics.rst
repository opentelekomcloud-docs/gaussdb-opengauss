:original_name: opengauss_01_0073.html

.. _opengauss_01_0073:

Viewing Monitoring Metrics
==========================

Scenarios
---------

Cloud Eye monitors operating statuses of DB instances. You can view the DB instance monitoring metrics on the management console. For details, see :ref:`Viewing Metrics of DB Instances <en-us_topic_0000002088677266__en-us_topic_0192954070_section3645894911344>`.

Monitored data takes some time for transmission and display. The DB instance status displayed on the Cloud Eye console is the status of the last 5 to 10 minutes. If your DB instance is newly created, wait for 5 to 10 minutes and then view the monitoring data.

Prerequisites
-------------

-  A DB instance is running properly.

   Monitoring metrics of the DB instances that are faulty or have been deleted cannot be displayed on the Cloud Eye console. You can view their monitoring metrics after they are rebooted or restored to be normal.

.. note::

   If a DB instance has been faulty for 24 hours, Cloud Eye considers that it does not exist and deletes it from the monitoring object list. You need to manually clear the alarm rules created for the DB instance.

-  The DB instance keeps running properly for about 10 minutes.

   For a newly created DB instance, you need to wait for a while before viewing the monitoring metrics.

.. _en-us_topic_0000002088677266__en-us_topic_0192954070_section3645894911344:

Viewing Metrics of DB Instances
-------------------------------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. In the service list, click **Cloud Eye** under **Management & Deployment**.

#. In the navigation pane on the left, choose **Cloud Service Monitoring** > **GaussDB**.

#. Click the target instance name to view its monitoring information.

   Cloud Eye can monitor performance metrics in the last 1 hour, last 3 hours, last 12 hours, last 1 day, or last 7 days.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
