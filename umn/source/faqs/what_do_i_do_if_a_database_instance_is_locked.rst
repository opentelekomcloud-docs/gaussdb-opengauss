:original_name: opengauss_faq_0002.html

.. _opengauss_faq_0002:

What Do I Do If a Database Instance Is Locked?
==============================================

#. On the **Instances** page, click the instance name to go to the **Basic Information** page.

#. In the navigation pane on the left, click **Parameters**.

#. Set **password_lock_time** to **0** and **failed_login_attempts** to **0** to unlock the database.

   To ensure database security, change the parameter values to the default values after the password is reset.

#. In the **DB Information** area, click **Reset Password** next to the **Administrator** field.

#. Enter a new password and confirm the password.

#. Change the values of **password_lock_time** and **failed_login_attempts** to the default values.
