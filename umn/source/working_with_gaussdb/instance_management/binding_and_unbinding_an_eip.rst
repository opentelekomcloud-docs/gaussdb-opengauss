:original_name: opengauss_01_0037.html

.. _opengauss_01_0037:

Binding and Unbinding an EIP
============================

Scenarios
---------

You can bind an EIP to a GaussDB instance for public access and can unbind the EIP from an instance as required.

.. important::

   To ensure that the database is accessible, the security group used by the database must allow access to related ports. For example, if the database port is 1611, ensure that the security group allows inbound traffic on the specified ports or port ranges: 1611 to 1711, 20050, 5000 and 5001, 2379 and 2380, 6000, 6500, as well as 40000 to 60480.

Precautions
-----------

-  If a DB instance has already been bound with an EIP, you must unbind the EIP from the instance first before binding a new EIP to it.
-  An EIP can be bound to only one DB instance.

.. _en-us_topic_0000002088677450__en-us_topic_0204853970_en-us_topic_0192953870_en-us_topic_0192953725_section3199593620428:

Binding an EIP
--------------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. On the **Instances** page, click the name of the target instance to go to the **Basic Information** page.

#. In the **Connection Information** area, locate the private IP address and click **Bind** in the **Operation** column.

#. In the displayed dialog box, all unbound EIPs are listed. Select the EIP to be bound and click **OK**. If no available EIPs are displayed, click **View EIP** and obtain an EIP.

#. In the **Connection Information** area, view the bound IP address.

   To unbind the EIP from the instance, see :ref:`Unbinding an EIP <en-us_topic_0000002088677450__en-us_topic_0204853970_en-us_topic_0192953870_en-us_topic_0192953725_section186511510267>`.

   .. note::

      After the EIP is bound, you can click |image3| next to the private IP address to view its details.

.. _en-us_topic_0000002088677450__en-us_topic_0204853970_en-us_topic_0192953870_en-us_topic_0192953725_section186511510267:

Unbinding an EIP
----------------

#. Log in to the management console.

#. Click |image4| in the upper left corner and select a region and project.

#. Click |image5| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. On the **Instances** page, click the instance that has been bound with an EIP.

#. In the **Connection Information** area, locate the EIP that you want to unbind and click **Unbind** in the **Operation** column. In the displayed dialog box, click **Yes**.

#. In the **Connection Information** area, view the results.

   To bind an EIP to the instance again, see :ref:`Binding an EIP <en-us_topic_0000002088677450__en-us_topic_0204853970_en-us_topic_0192953870_en-us_topic_0192953725_section3199593620428>`.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
.. |image3| image:: /_static/images/en-us_image_0000002088518010.png
.. |image4| image:: /_static/images/en-us_image_0000002088517922.png
.. |image5| image:: /_static/images/en-us_image_0000002124197217.png
