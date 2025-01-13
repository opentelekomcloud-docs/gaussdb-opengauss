:original_name: opengauss_recycle.html

.. _opengauss_recycle:

Rebuilding a Deleted Instance
=============================

You can rebuild a deleted DB instance from the recycle bin.

The recycle bin is enabled by default and cannot be disabled. The retention period is 7 days by default.

Modifying the Recycling Policy
------------------------------

.. important::

   You can modify the retention period, and the changes only apply to the DB instances deleted after the changes, so exercise caution when performing this operation.

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and project.
#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.
#. In the navigation pane on the left, choose **Recycle Bin**.
#. Click **Modify Recycling Policy**. In the displayed dialog box, set the retention period for the deleted DB instances from 1 day to 7 days.
#. Click **OK**.

Rebuilding a DB Instance
------------------------

You can rebuild DB instances in the recycle bin within the retention period.

#. Log in to the management console.
#. Click |image3| in the upper left corner and select a region and project.
#. Click |image4| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.
#. In the navigation pane on the left, choose **Recycle Bin**.
#. Locate the DB instance to be rebuilt and click **Rebuild** in the **Operation** column.
#. On the displayed page, configure required parameters and submit the task.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
.. |image3| image:: /_static/images/en-us_image_0000002088517922.png
.. |image4| image:: /_static/images/en-us_image_0000002124197217.png
