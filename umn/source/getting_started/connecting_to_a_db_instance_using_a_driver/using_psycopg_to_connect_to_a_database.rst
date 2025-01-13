:original_name: opengauss_Psycopg_connect.html

.. _opengauss_Psycopg_connect:

Using Psycopg to Connect to a Database
======================================

Psycopg is a Python API used to execute SQL statements and provides a unified access API for PostgreSQL and GaussDB databases. It is designed for applications that perform data operations. Psycopg 2 is mostly implemented in C as a libpq wrapper, which is efficient and secure. It provides client-side and server-side cursors, asynchronous communication and notification, and COPY TO and COPY FROM functions. Many Python types are out-of-the-box and adapted to matching PostgreSQL data types. Adaptation can be extended and customized with a flexible object adaptation system. Psycopg2 is compatible with Unicode and Python 3.

GaussDB supports Psycopg2 features and allows Psycopg2 to be connected in SSL mode.

.. table:: **Table 1** Platforms supported by Psycopg

   ================ ========
   Operating System Platform
   ================ ========
   EulerOS 2.5      x86_64
   EulerOS 2.8      ARM64
   ================ ========

Obtaining the Driver Package
----------------------------

`Download <https://dbs-download.obs.otc.t-systems.com/rds/GaussDB_opengauss_client_tools.zip>`__ the GaussDB driver package **GaussDB_opengauss_client_tools.zip**.

Prerequisites
-------------

-  A Python development environment has been installed on the local PC.

-  You have obtained and decompressed the Python driver package. The package contains the following folders:

   -  **psycopg2**: **psycopg2** library file
   -  **lib**: **lib** library file.

-  Before using the driver, perform the following operations:

   #. Decompress the driver package of the corresponding version and copy psycopg2 to the **site-packages** folder in the Python installation directory as user **root**.
   #. Change the **psycopg2** directory permission to **755**.
   #. Add the **psycopg2** directory to the environment variable *$PYTHONPATH* and validate it.
   #. For non-database users, add the **lib** directory after decompression to *LD_LIBRARY_PATH*.

-  Load the following database driver before creating a database connection.

   .. code-block::

      import  psycopg2

Connecting a Database
---------------------

#. Use the **.ini** file (the **configparser** package of Python can parse this type of configuration file) to save the configuration information about the database connection.
#. Use the **psycopg2.connect** function to obtain the connection object.
#. Use the connection object to create a cursor object.
