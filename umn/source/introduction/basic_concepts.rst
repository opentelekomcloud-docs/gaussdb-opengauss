:original_name: opengauss_01_0003.html

.. _opengauss_01_0003:

Basic Concepts
==============

DB Instances
------------

DB instances are the smallest management units used by GaussDB(openGauss). Each is an isolated database environment on the cloud. You can create and manage DB instances on the management console. For details about DB instance types, specifications, engines, versions, and statuses, see :ref:`DB Instance Description <opengauss_01_0005>`.

Instance Versions
-----------------

Currently, only 2020 Enterprise Edition is supported.

DB Instance Types
-----------------

GaussDB(openGauss) supports distributed DB instances. Distributed DB instances support capability expansion through shards to cope with highly concurrent access to massive volumes of data. Primary/standby DB instances are suitable for scenarios with small and stable volumes of data and that have high requirements on data reliability and service availability.

DB Instance Classes
-------------------

DB instance classes consist of vCPUs and memory (GB). For details, see :ref:`DB Instance Classes <opengauss_01_0007>`.

Automated Backups
-----------------

When you create a DB instance, the automated backup function is enabled by default. After the DB instance is created, you can modify the backup policy. GaussDB(openGauss) will automatically create backups for DB instances based on your settings.

Manual Backups
--------------

Manual backups are user-initiated full backups of DB instances. They are retained until you delete them manually.

Regions and AZs
---------------

A region and availability zone (AZ) identify the location of a data center. You can create resources in a specific region and AZ.

-  A region is a physical data center. Each region is completely independent, improving fault tolerance and stability. The region that is selected during resource creation cannot be changed after the resource is created.
-  An AZ is a physical location using independent power supplies and networks. Faults in an AZ do not affect other AZs. A region can contain multiple AZs, which are physically isolated but interconnected through internal networks. This ensures the independence of AZs and provides low-cost and low-latency network connections.

:ref:`Figure 1 <opengauss_01_0003__fig2922183194716>` shows the relationship between regions and AZs.

.. _opengauss_01_0003__fig2922183194716:

.. figure:: /_static/images/en-us_image_0000001072470721.png
   :alt: **Figure 1** Regions and AZs

   **Figure 1** Regions and AZs

Projects
--------

Projects are used to group and isolate OpenStack resources (computing resources, storage resources, and network resources). A project can be a department or a project team. Multiple projects can be created for one account.
