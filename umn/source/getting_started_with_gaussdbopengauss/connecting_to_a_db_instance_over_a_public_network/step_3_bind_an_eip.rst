:original_name: opengauss_01_0028.html

.. _opengauss_01_0028:

Step 3: Bind an EIP
===================

Scenarios
---------

By default, a GaussDB(openGauss) DB instance without an EIP bound to it is not accessible over the public network. You can bind an EIP to a GaussDB(openGauss) DB instance for public access and can unbind the EIP from it when necessary.

Precautions
-----------

-  Before accessing the DB instance, you need to add an individual IP address or IP address range to the inbound rule. For details, see :ref:`Step 3: Configure Security Group Rules <opengauss_01_0029>`.

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

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
.. |image2| image:: /_static/images/en-us_image_0000001072758914.png
