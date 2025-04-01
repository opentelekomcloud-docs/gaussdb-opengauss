:original_name: opengauss_trial.html

.. _opengauss_trial:

Example: Using DAS to Connect to an Instance and Execute SQL Statements
=======================================================================

This section describes how to create a GaussDB instance with the minimum specifications and execute basic SQL syntax.

Creating an Instance
--------------------

#. Log in to the console.

#. Click |image1| in the upper left corner and select a region.

#. Click |image2| on the left and choose **Databases** > **GaussDB**.

#. On the **Instances** page, click **Create DB Instance**.

#. Configure the basic information, such as the instance name.

#. Configure instance specifications.

#. Select a VPC and security group for the instance and configure the database port.

   The VPC you selected must contain sufficient subnets.

#. Configure the administrator password and parameter template.

#. Click **Next**. On the displayed page, confirm the information and click **Submit**.

#. Go to the instance list.

   If status of the instance becomes available, the instance has been created.

Connecting to an Instance Through DAS
-------------------------------------

#. Log in to the console.

#. Click |image3| in the upper left corner and select a region.

#. Click |image4| on the left and choose **Databases** > **Data Admin Service**.

#. In the navigation pane on the left, click **Development Tool** to go to the login list page.

#. Click **Add Login**.

   .. important::

      After the database is created, the **root** user is added by default. You do not need to create a **root** user.

#. Set **DB Engine** to **GaussDB**, retain the default value of **Source Database**, and configure required parameters.

   You are advised to enable **Collect Metadata Periodically** and **Show Executed SQL Statements**.

   If a message is displayed indicating that a connection has been established, go to :ref:`9 <en-us_topic_0000002124197133__li1796385542016>`.

#. Click **Test Connection**.

   If a message is displayed indicating connection successful, continue with the operation. If a message is displayed indicating connection failed and the failure cause is provided, make modifications according to the error message.

#. Click **OK**.

#. .. _en-us_topic_0000002124197133__li1796385542016:

   Locate the added instance, click **Log In** in the **Operation** column.

#. Access the SQL query page.

Getting Started with SQL
------------------------

#. Create a database user.

   Only administrators that are created during the instance installation can access the initial database by default. You can also create other database users.

   **CREATE USER** *joe* **WITH** **PASSWORD "**\ xxxxxxxxx\ **";**

   If information similar to the following is displayed, the user has been created.

   |image5|

   In this case, you have created a user named **joe**, and the user password is **xxxxxxx**.

#. Create a database.

   **CREATE DATABASE** *db_tpcds*;

   If information similar to the following is displayed, the database has been created.

   |image6|

   Switch to the newly created database in the upper left corner.

   |image7|

#. Create a table.

   -  Run the following command to create a schema:

      **CREATE SCHEMA** *myschema*\ **;**

   -  Create a table named **mytable** that has only one column. The column name is **firstcol** and the column type is **integer**.

      **CREATE TABLE** myschema\ *.*\ mytable *(firstcol int)*;

   -  Insert data to the table.

      **INSERT INTO** myschema\ *.*\ mytable values (100);

   -  View data in the table.

      **SELECT \* FROM** myschema.mytable;

   **Note:**

   -  By default, new database objects, such as the **mytable** table, are created in the *$user* schema.
   -  In addition to the created tables, a database contains many system catalogs. These system catalogs contain cluster installation information and information about various queries and processes in GaussDB. You can collect information about the database by querying the system catalogs.

#. In the **db_tpcds** database, run the following statement as user **root** to grant all permissions of the **db_tpcds** database to user **joe**:

   **GRANT ALL ON DATABASE** db_tpcds **TO** joe;

   **GRANT USAGE ON schema** myschema **TO** joe\ *;*

   **GRANT ALL ON TABLE** myschema.mytable **TO** joe;

#. Log in to the **db_tpcds** database as user **joe**.

#. After login, insert data into the table and verify the data.

   **INSERT INTO** myschema.mytable values (200);

   **SELECT \* FROM** myschema.mytable;

   |image8|

.. |image1| image:: /_static/images/en-us_image_0000002088518414.png
.. |image2| image:: /_static/images/en-us_image_0000002124277613.png
.. |image3| image:: /_static/images/en-us_image_0000002124278013.png
.. |image4| image:: /_static/images/en-us_image_0000002124277613.png
.. |image5| image:: /_static/images/en-us_image_0000002124197713.png
.. |image6| image:: /_static/images/en-us_image_0000002088518426.png
.. |image7| image:: /_static/images/en-us_image_0000002124197717.png
.. |image8| image:: /_static/images/en-us_image_0000002088678282.png
