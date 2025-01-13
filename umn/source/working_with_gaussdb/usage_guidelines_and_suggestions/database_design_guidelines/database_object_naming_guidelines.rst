:original_name: opengauss_tips_0008.html

.. _opengauss_tips_0008:

Database Object Naming Guidelines
=================================

-  The name of a database object must contain up to 63 characters, start with a letter or underscore (_), and can contain letters, digits, underscores (_), dollar signs ($), and number signs (#).

-

   .. note::

      Run the following command to view the keywords:

      **SELECT** \* **FROM** pg_get_keywords();

-  Do not use a string enclosed in double quotation marks ("") to define the database object name, unless you need to specify its capitalization.

   .. note::

      For GaussDB, the object name is case-insensitive by default in SQL statements. If two different tables (**t_Table** and **t_table**) exist in the same database, use double quotation marks (""). Otherwise, do not use double quotation marks (""). Case sensitivity of database object names makes problem location difficult.

-  The naming style of database objects must be consistent. Lowercase letters are recommended.

   In a system undergoing incremental development or service migration, comply with its historical naming conventions.

   Use multiple words separated with underscores (_).

   Use intelligible names and common acronyms or abbreviations for database objects. Acronyms or abbreviations that are generally understood are recommended.

   A variable name must be descriptive and meaningful. It must have a prefix indicating its type.

   .. table:: **Table 1** Naming rules of database objects

      +-------------------+-------------+-------------------------+--------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Object Type       | Prefix      | Example                 | Length Restriction | Remarks                                                                                                                                                                           |
      |                   |             |                         |                    |                                                                                                                                                                                   |
      |                   |             |                         | **(Unit: Byte)**   |                                                                                                                                                                                   |
      +===================+=============+=========================+====================+===================================================================================================================================================================================+
      | Database name     | db\_        | db_businessname         | <=63               | /                                                                                                                                                                                 |
      +-------------------+-------------+-------------------------+--------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Common table      | t\_         | t_tablename             | <=63               | /                                                                                                                                                                                 |
      +-------------------+-------------+-------------------------+--------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Temporary table   | tmp\_       | tmp_tablename           | <=63               | For example, an intermediate table used by operation personnel for temporary backup or data collection.                                                                           |
      |                   |             |                         |                    |                                                                                                                                                                                   |
      |                   |             |                         |                    | Naming rule: **tmp\_**\ *Table name abbreviation*\ **\_**\ *Creator account abbreviation*\ **\_**\ *Creation date*, for example, **tmp_user_ytw_160505**.                         |
      +-------------------+-------------+-------------------------+--------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Primary key       | pk\_        | pk_tablename            | <=63               | If the table name is too long, use the abbreviation of the table name. Use the universal abbreviation or the abbreviation without vowels.                                         |
      +-------------------+-------------+-------------------------+--------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | UNIQUE constraint | uk\_        | uk_tablename_columnname | <=63               | Unique index, which is named **uk\_**.                                                                                                                                            |
      |                   |             |                         |                    |                                                                                                                                                                                   |
      |                   |             |                         |                    | If the table name or field name is too long, use the abbreviation of the table name or field name. Use the universal abbreviation or the abbreviation without vowels.             |
      +-------------------+-------------+-------------------------+--------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Function          | f\_         | f_functionname          | <=63               | /                                                                                                                                                                                 |
      +-------------------+-------------+-------------------------+--------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Table field       | /           | /                       | <=63               | Use meaningful English words or abbreviations to name fields.                                                                                                                     |
      |                   |             |                         |                    |                                                                                                                                                                                   |
      |                   |             |                         |                    | For example, a field of the bool type is named in the format of **is\_**\ *Description*. A column whose member status is **enabled** in the member table is named **is_enabled**. |
      +-------------------+-------------+-------------------------+--------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
