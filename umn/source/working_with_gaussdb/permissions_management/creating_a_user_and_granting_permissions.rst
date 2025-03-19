:original_name: opengauss_07_0002.html

.. _opengauss_07_0002:

Creating a User and Granting Permissions
========================================

This section describes how to use Identity and Access Management (IAM) for fine-grained permissions management for your GaussDB resources. With IAM, you can:

-  Create IAM users for employees based on your enterprise's organizational structure. Each IAM user will have their own security credentials for accessing GaussDB resources.
-  Grant only the permissions required for users to perform a specific task.
-  Entrust an account or cloud service to perform efficient O&M on your GaussDB resources.

If your account does not require individual IAM users, skip this section.

:ref:`Figure 1 <en-us_topic_0000002088677622__en-us_topic_0172661625_fig15451536531>` describes the process for granting permissions.

Prerequisites
-------------

Before assigning permissions to user groups, you should learn about the system permissions listed in the section "Service Overview" > "Permissions Management". For the system policies of other services, see the section "System Permission".

Process Flow
------------

.. _en-us_topic_0000002088677622__en-us_topic_0172661625_fig15451536531:

.. figure:: /_static/images/en-us_image_0000002088518246.png
   :alt: **Figure 1** Process of granting GaussDB permissions

   **Figure 1** Process of granting GaussDB permissions

#. .. _en-us_topic_0000002088677622__en-us_topic_0172661625_li10176121316284:

   Create a user group on the IAM console, and attach the **GaussDB ReadOnlyAccess** policy to the group.

#. Create a user on the IAM console and add the user to the group created in :ref:`1 <en-us_topic_0000002088677622__en-us_topic_0172661625_li10176121316284>`.

#. Log in and verify permissions.

   Log in to the console by using the created user, and verify that the user only has read permissions for GaussDB.

   -  Under the service list, choose GaussDB. In the navigation pane on the left, choose **GaussDB** > **Instances**. Click **Buy DB Instance** in the upper right corner. If a message appears indicating that you have insufficient permissions to perform the operation, the GaussDB ReadOnlyAccess policy has already taken effect.
   -  Choose any other service in the service list. If a message appears indicating that you have insufficient permissions to access the service, the **GaussDB ReadOnlyAccess** policy has already taken effect.
