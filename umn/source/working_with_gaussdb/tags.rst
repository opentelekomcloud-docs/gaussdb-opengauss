:original_name: opengauss_tag.html

.. _opengauss_tag:

Tags
====

Scenarios
---------

Tag Management Service (TMS) enables you to use tags on the management console to manage resources. TMS works with other cloud services to manage tags. TMS manages tags globally, and other cloud services manage their own tags.

-  You are advised to set predefined tags on the TMS console.
-  A tag consists of a key and value. You can add only one value for each key.
-  Each instance can have up to 20 tags.

Adding a Tag
------------

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and project.
#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.
#. On the **Instances** page, click the instance name to go to the **Basic Information** page.
#. In the navigation pane on the left, click **Tags**. On the **Tags** page, click **Add Tag**. In the displayed dialog box, enter a tag key and a tag value, and click **OK**.

   -  The tag key must be unique and consist of 1 to 36 characters. Only letters, digits, hyphens (-), underscores (_), and at signs (@) are allowed.
   -  Tag value (optional) can contain up to 43 characters. Only letters, digits, hyphens (-), underscores (_), at signs (@), and periods (.) are allowed.

#. View and manage the tag on the **Tags** page.

Editing a Tag
-------------

#. Log in to the management console.
#. Click |image3| in the upper left corner and select a region and project.
#. Click |image4| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.
#. On the **Instances** page, click the instance name to go to the **Basic Information** page.
#. In the navigation pane on the left, click **Tags**, locate the tag to be edited, and click **Edit** in the **Operation** column.
#. In the displayed dialog box, enter a tag value and click **OK**.

   -  Only the tag value can be edited.
   -  Tag value (optional) can contain up to 43 characters. Only letters, digits, hyphens (-), underscores (_), at signs (@), and periods (.) are allowed.

#. View and manage the tag on the **Tags** page.

Deleting a Tag
--------------

#. Log in to the management console.
#. Click |image5| in the upper left corner and select a region and project.
#. Click |image6| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.
#. On the **Instances** page, click the instance name to go to the **Basic Information** page.
#. In the navigation pane on the left, click **Tags**, locate the tag to be deleted, and click **Delete** in the **Operation** column.
#. In the displayed dialog box, click **Yes**.
#. Check that the tag is no longer displayed on the **Tags** page.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
.. |image3| image:: /_static/images/en-us_image_0000002088517922.png
.. |image4| image:: /_static/images/en-us_image_0000002124197217.png
.. |image5| image:: /_static/images/en-us_image_0000002088517922.png
.. |image6| image:: /_static/images/en-us_image_0000002124197217.png
