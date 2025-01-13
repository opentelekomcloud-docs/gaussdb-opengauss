:original_name: opengauss_01_0019.html

.. _opengauss_01_0019:

Connecting to an Instance Through DAS
=====================================

Scenarios
---------

DAS enables you to manage your databases from a web-based console. It supports SQL execution, advanced database management, and intelligent O&M, simplifying database management and improving both efficiency and data security.

Procedure
---------

#. Log in to the management console.

#. Click |image1| in the upper left corner and select a region and project.

#. Click |image2| in the upper left corner of the page and choose **Databases** > **Data Admin Service**.

#. On the DAS console that is displayed, locate the instance you want to log in to and click **Log In** in the **Operation** column.

   Alternatively, click the DB instance name on the **Instances** page. On the displayed **Basic Information** page, click **Log In** in the upper right corner of the page.

#. On the displayed login page, enter the username and password and click **Log In**.

   .. table:: **Table 1** Parameter description

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                                         |
      +===================================+=====================================================================================================================================================================================================================================================================================================================+
      | Login Username                    | Username of the GaussDB database account. The default administrator is **root**.                                                                                                                                                                                                                                    |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Database Name                     | Name of the database to connect to. The default management database is **postgres**.                                                                                                                                                                                                                                |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Password                          | Password of the database user.                                                                                                                                                                                                                                                                                      |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Collect Metadata Periodically     | You are advised to enable **Collect Metadata Periodically**. If it is disabled, DAS obtains only the structured data from databases in real time, and the performance of databases is affected.                                                                                                                     |
      |                                   |                                                                                                                                                                                                                                                                                                                     |
      |                                   | The collection time cannot be customized. Once **Collect Metadata Periodically** is enabled, DAS collects metadata at 20:00 every day (UTC time). If you are not using a UTC time, convert the time according to your local time zone. You can also click **Collect Now** to collect metadata at any time you want. |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Show Executed SQL Statements      | You are advised to enable **Show Executed SQL Statements**. With it enabled, you can view the executed SQL statements under **SQL Operations** > **SQL History** and execute them again without entering the SQL statements.                                                                                        |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124277857.png
