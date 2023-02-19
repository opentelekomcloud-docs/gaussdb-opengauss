:original_name: opengauss_01_0039.html

.. _opengauss_01_0039:

Adding Nodes
============

Scenarios
---------

As the DB instance deployment time and services increase, the database running performance and storage will gradually reach the bottleneck. For GaussDB(openGauss), you can add nodes to improve the DB instance performance and storage capability.

.. important::

   The number of CNs of a DB instance must be less than or equal to twice the number of shards.

**Procedure**
-------------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and a project.

#. Click **Service List**. Under **Database**, click **GaussDB**.

#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.

#. On the **Instance Management** page, click the DB instance for which you want to add nodes.

#. On the **Basic Information** page, add nodes.

   **Adding shards**

   a. Click **Add** next to **Shards**.
   b. Specify the number of shards to be added. Click **Next**.
   c. Confirm the specifications and fees, and then click **Submit**.

   .. note::

      A shard contains three DN replicas by default. Therefore, three DN replicas are added each time a shard is added.

   **Adding CNs**

   a. Click **Add** next to **Coordinator Nodes**.

   b. Specify the number of coordinator nodes to be added and the AZ.

      If only one AZ is specified during instance creation, the same AZ must be selected for adding CNs.

   c. Click **Next**.

   d. Confirm the specifications and click **Submit**.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
