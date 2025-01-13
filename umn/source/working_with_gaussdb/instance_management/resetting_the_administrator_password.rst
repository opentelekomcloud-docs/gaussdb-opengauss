:original_name: opengauss_01_0036.html

.. _opengauss_01_0036:

Resetting the Administrator Password
====================================

Scenarios
---------

If you forget the password of your **root** account when using GaussDB, you can reset the password.

Precautions
-----------

-  If the password you provide is regarded as a weak password by the system, you will be prompted to enter a stronger password.

-  If the DB instance is abnormal, the administrator password cannot be reset.
-  The volume of data being processed by the instance determines how long it takes for the new password to take effect.
-  To prevent brute force cracking and ensure system security, change your password periodically.

Procedure
---------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.

#. On the **Instances** page, locate the instance that you want to reset password for and click **More** > **Reset Password** in the **Operation** column.

   Alternatively, click the instance name to go to the **Basic Information** page. In the **DB Information** area, click **Reset Password** next to the **Administrator** field.

#. Enter a new password and confirm the password.

   .. important::

      The new password must meet the following requirements:

      -  Contains 8 to 32 characters.
      -  Contains at least three types of the following: uppercase letters, lowercase letters, digits, and special characters (``~!@#%^*-_=+?,``).
      -  Cannot be the same as the old password.

   -  To submit the new password, click **OK**.
   -  To cancel the operation, click **Cancel**.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
