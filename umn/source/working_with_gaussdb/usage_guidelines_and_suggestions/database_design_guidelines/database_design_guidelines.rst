:original_name: opengauss_tips_0009.html

.. _opengauss_tips_0009:

Database Design Guidelines
==========================

-  The database name must be specified when the JDBC client is used to connect to the database. The format is as follows:

   .. code-block::

      jdbc:postgresql://host:port/database?param1=value1&param2=value2

-  Once a JDBC instance is created, the database cannot be switched.

-  The database does not support case-insensitive collation.

-  Currently, character sets can be defined only for databases.

-  Before using an application, you need to create a database for it.

   .. note::

      Do not use the Postgres database created by default after the database is installed to store data.

-  When creating a database, you must set the character set to **UTF8** and must select the encoding character set that is the same as that of the client.

   To meet globalization requirements, database encoding should store and identify most characters. Therefore, UTF8 is recommended. The UTF8 character set in GaussDB is equivalent to the UTF8MB4 character set in MySQL and supports emoji.

   If the encoding mode of the client is different from that of the database, the transcoding performance is affected. In addition, kernel optimization for the same encoding cannot be triggered, affecting the query efficiency.

   To change the encoding character set of the client, perform the following steps:

   -  Configure client connection parameters. Take JDBC as an example. Add **characterEncoding** and **allowEncodingChanges** to the URL.

      .. code-block::

         jdbc:postgresql://ip:port/database_name?characterEncoding=utf8&allowEncodingChanges=true

   -  Modify the database GUC parameter.

      .. code-block::

         SET client_encoding = 'UTF8';

   -  Configure the database encoding when executing the **CREATE DATABASE**.

      .. code-block::

         CREATE DATABASE tester WITH ENCODING = 'UTF8';

-  The character set cannot be changed once the database is created.

-  You are advised to use schemas to isolate workloads for convenience and resource sharing.

   .. note::

      The DATABASE and SCHEMA modes can be used to isolate workloads.

      The difference is that the database isolation is more thorough. The shared resources between databases are few, and connection isolation and permission isolation can be implemented.

      However, the databases cannot access each other. The database must be specified during JDBC connection establishment. After the connection is established, the database cannot be switched.

      Schemas share more resources than databases do. User permissions on schemas and subordinate objects can be controlled using the **GRANT** and **REVOKE** syntax.

-  When creating a database, you are advised to set **LC_COLLATE** and **LC_CTYPE** to the language (for example, English) of the stored data. This parameter affects the data collation sequence. By default, the default settings of the current environment variables are used.

   Example:

   .. code-block::

      CREATE DATABASE tester WITH ENCODING = 'UTF8' LC_COLLATE = 'en_US.UTF-8' LC_CTYPE = 'en_US.UTF-8';

   **LC_COLLATE**: specifies the character collation rule.

   .. code-block::

      LC_COLLATE=C
      1
      2
      3
      A
      B
      C
      a -- Note: Lowercase letters are placed after uppercase letters and sorted in ASCII mode.
      b
      c

      en_US.UTF-8
      1
      2
      3
      a -- Note: Sorted by character.
      A
      b
      B
      c
      C

      zh_CN.UTF-8
      1
      2
      3
      a
      A
      b
      B
      c
      C

   **LC_CTYPE**: specifies which data is alpha (**is_alpha**), uppercase (**is_upper**), or lowercase letter (**is_lower**).
