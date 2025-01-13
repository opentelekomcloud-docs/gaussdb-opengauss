:original_name: opengauss_01_0039.html

.. _opengauss_01_0039:

Adding Nodes
============

Scenarios
---------

As the instance deployment time and data increase, the database performance and storage will gradually reach the bottleneck. Adding nodes can improve the instance performance and storage capacity.

.. important::

   You can flexibly add CNs or shards as needed. It is recommended that the number of CNs of a GaussDB instance do not exceed twice the number of shards.

   -  Instances can be scaled out only when they are in the **Available** state. When shards are being added, you can still query and insert data, query services are not interrupted, and the data insertion performance is not affected. The performance of join queries on local tables across node groups during redistribution may be affected.

**Procedure**
-------------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. On the **Instances** page, click the name of the instance for which you want to add nodes.

#. In the **DB Information** area of the **Basic Information** page, add shards or CNs.

   **Adding shards**

   a. Click **Add** next to **Shards**.
   b. Specify the number of shards to be added. Click **Next**.
   c. Confirm the information and then click **Submit**.

   .. note::

      By default, a shard contains three replicas (a primary DN and two standby DNs). Each time you add a shard, three replicas will be added.

   **Adding CNs**

   a. Click **Add** next to **Coordinator Nodes**.

   b. Specify the number of coordinator nodes to be added and the AZ.

      If single-AZ deployment is specified during the instance creation, CNs are only added to the AZ you specified.

   c. Click **Next**.

   d. Confirm the information and click **Submit**.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
