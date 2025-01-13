:original_name: opengauss_01_0003.html

.. _opengauss_01_0003:

Basic Concepts
==============

Instances
---------

The smallest management unit in GaussDB is the instance. A DB instance is an isolated database environment hosted in the cloud. You can create and manage GaussDB instances on the management console. For details about instance statuses, instance specifications, storage types, and versions, see :ref:`DB Instance Description <opengauss_01_0005>`.

Instance Versions
-----------------

Currently, GaussDB 8.103, 3.223, 3.222, 3.208, 3.103, 2.7, 2.3, 1.4, and 1.0 are supported.

Instance Types
--------------

GaussDB supports distributed instances. Distributed instances allow you to add nodes as needed to handle large volumes of concurrent requests. The primary/standby instances are suitable for scenarios with small and stable volumes of data, where data reliability and service availability are important.

Instance Specifications
-----------------------

The instance specifications determine the computation (vCPUs) and memory capacity (in GB) of an instance. For details, see :ref:`Instance Specifications <opengauss_01_0007>`.

Coordinator Nodes
-----------------

Coordinator nodes (CNs) store database metadata, distribute and execute query tasks, and then return the query results from DNs to applications.

Data Nodes
----------

Data nodes (DNs) store and query table data.

Automated Backups
-----------------

When you create an instance, automated backup is enabled by default. After the instance is created, you can modify the backup policy. GaussDB will automatically create backups for instances based on your settings.

Manual Backups
--------------

Manual backups are user-initiated full backups of instances. They are retained until you delete them manually.

Regions and AZs
---------------

A region and availability zone (AZ) identify the location of a data center. You can create resources in a specific region and AZ.

-  A region is a physical data center. Each region is isolated from the other regions, improving fault tolerance and stability. The region that is selected during resource creation cannot be changed after the resource is created.
-  An AZ is a physical location where resources use independent power supplies and networks. Within a region, there can be multiple AZs that are physically isolated from each other but interconnected through internal networks. This ensures the independence of each AZ while also providing low-cost and low-latency network connectivity.

:ref:`Figure 1 <en-us_topic_0000002124277413__fig2922183194716>` shows the relationship between regions and AZs.

.. _en-us_topic_0000002124277413__fig2922183194716:

.. figure:: /_static/images/en-us_image_0000002088518190.png
   :alt: **Figure 1** Regions and AZs

   **Figure 1** Regions and AZs
