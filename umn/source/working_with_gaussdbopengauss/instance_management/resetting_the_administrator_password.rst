:original_name: opengauss_01_0036.html

.. _opengauss_01_0036:

Resetting the Administrator Password
====================================

Scenarios
---------

You can reset the administrator password of a GaussDB(openGauss) DB instance.

If you forget the password of your database account when using GaussDB(openGauss), you can reset it.

Precautions
-----------

-  If you have changed the password of the primary node, the password of the standby node will also be changed accordingly.
-  The volume of data being processed by the primary node determines how long it takes for the new password to take effect.
-  To prevent brute force cracking and ensure system security, change your password periodically.

Method 1
--------

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and a project.
#. Click **Service List**. Under **Database**, click **GaussDB**.
#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.
#. On the **Instance Management** page, locate the target DB instance and click **More** > **Reset Password** in the **Operation** column.
#. Enter a new password and confirm the password.

   -  To submit the new password, click **OK**.
   -  To cancel the reset operation, click **Cancel**.

   .. important::

      Keep your password secure because you cannot retrieve it from the system.

      The new password must meet the following requirements:

      -  Contains 8 to 32 characters.
      -  Contains at least three types of the following: uppercase letters, lowercase letters, digits, and special characters (``~!@#%^*-_=+?,``).

Method 2
--------

#. Log in to the management console.
#. Click |image2| in the upper left corner and select a region and a project.
#. Click **Service List**. Under **Database**, click **GaussDB**.
#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.
#. On the **Instance Management** page, click the target DB instance.
#. In the **DB Information** area on the **Basic Information** page, click **Reset Password** in the **Administrator** field.
#. Enter a new password and confirm the password.

   -  To submit the new password, click **OK**.
   -  To cancel the reset operation, click **Cancel**.

   .. important::

      Keep your password secure because you cannot retrieve it from the system.

      The new password must meet the following requirements:

      -  Contains 8 to 32 characters.
      -  Contains at least three types of the following: uppercase letters, lowercase letters, digits, and special characters (``~!@#%^*-_=+?,``).

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
.. |image2| image:: /_static/images/en-us_image_0000001072358973.png
