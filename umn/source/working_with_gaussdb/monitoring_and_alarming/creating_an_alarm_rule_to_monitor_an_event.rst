:original_name: opengauss_create_event.html

.. _opengauss_create_event:

Creating an Alarm Rule to Monitor an Event
==========================================

Scenarios
---------

You can create alarm rules for event monitoring.

Procedure
---------

#. Log in to the management console.
#. Click |image1| in the upper left corner of the page, and choose **Management & Governance** > **Cloud Eye**.
#. In the navigation pane, choose **Event Monitoring**. On the **Event Monitoring** page, click **Create Alarm Rule** and set required parameters.

   -  Specify **Name** and **Description**.
   -  Click |image2| next to the **Alarm Notification** field to enable alarm notification. The notification window is 24 hours by default. If the topics you need are not displayed in the **Notification Object** drop-down list, click **Create an SMN topic** first. Then, select **Generated alarm** and **Cleared alarm** for **Trigger Condition**.

      .. note::

         Cloud Eye sends notifications only within the notification window specified in the alarm rule.

#. Click **Create**.

.. |image1| image:: /_static/images/en-us_image_0000002124197737.png
.. |image2| image:: /_static/images/en-us_image_0000002088678294.png
