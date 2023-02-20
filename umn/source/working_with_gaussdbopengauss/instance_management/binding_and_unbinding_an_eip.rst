:original_name: opengauss_01_0037.html

.. _opengauss_01_0037:

Binding and Unbinding an EIP
============================

Scenarios
---------

You can bind an EIP to a GaussDB(openGauss) DB instance for public access and can unbind the EIP from a GaussDB(openGauss) DB instance as required.

.. important::

   To ensure that the database can be accessed, the security group used by the database must have the permission to access the database port. For example, if the database port is **1611**, ensure that the security group has the permission to access port **1611**.

Precautions
-----------

-  You can bind an EIP to a CN only.
-  If a DB instance has already been bound with an EIP, you must unbind the EIP from the DB instance first before binding a new EIP to it.

.. _opengauss_01_0037__s276bd2277a8e43d0b113e04386ea9290:

Binding an EIP
--------------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and a project.

#. Click **Service List**. Under **Database**, click **GaussDB**.

#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.

#. On the **Instance Management** page, click the target DB instance.

#. In the **Connection Information** area, click **Bind EIP**.

#. In the displayed dialog box, all unbound EIPs are listed. Select the EIP to be bound and click **OK**. If no available EIPs are displayed, click **View EIP** and obtain an EIP.

#. In the **Connection Information** area, view the bound IP address.

   To unbind the EIP from the DB instance, see :ref:`Unbinding an EIP <opengauss_01_0037__s7427bdddd2df4ea18f4047c25b4f3ebe>`.

   .. note::

      After the EIP is bound, you can click |image2| next to the private IP address to view its details.

.. _opengauss_01_0037__s7427bdddd2df4ea18f4047c25b4f3ebe:

Unbinding an EIP
----------------

#. Log in to the management console.

#. Click |image3| in the upper left corner and select a region and a project.

#. Click **Service List**. Under **Database**, click **GaussDB**.

#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.

#. On the **Instance Management** page, click the DB instance that has been bound with an EIP.

#. In the **Connection Information** area, click **Unbind** next to the IP address. In the displayed dialog box, click **Yes** to unbind the EIP.

#. In the **Connection Information** area, view the IP address.

   To bind an EIP to the DB instance again, see :ref:`Binding an EIP <opengauss_01_0037__s276bd2277a8e43d0b113e04386ea9290>`.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
.. |image2| image:: /_static/images/en-us_image_0000001072758914.png
.. |image3| image:: /_static/images/en-us_image_0000001072358973.png
