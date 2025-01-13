:original_name: opengauss_odbc_connect.html

.. _opengauss_odbc_connect:

Using ODBC to Connect to a Database
===================================

Open Database Connectivity (ODBC) is an application programming interface (API) for accessing databases based on the X/OPEN CLI. The ODBC API alleviates applications from directly operating in databases, and enhances the database portability, extensibility, and maintainability.

GaussDB supports ODBC 3.5. For details, see the following table.

.. table:: **Table 1** OSs supported by ODBC

   =================== ========
   Operating System    Platform
   =================== ========
   EulerOS 2.5         x86_64
   EulerOS 2.8         ARM64
   Windows 7           x86_32
   Windows 7           x86_64
   Windows Server 2008 x86_32
   Windows Server 2008 x86_64
   =================== ========

The ODBC Driver Manager running on UNIX or Linux can be unixODBC or iODBC. unixODBC-2.3.0 is used as the component for connecting the database.

Windows has a native ODBC Driver Manager. You can locate **Data Sources (ODBC)** by choosing **Control Panel > Administrative Tools**.

.. note::

   The current database ODBC driver is based on an open-source version and may be incompatible with data types, such as tinyint, smalldatetime, and nvarchar2.

Obtaining the Driver Package
----------------------------

`Download <https://dbs-download.obs.otc.t-systems.com/rds/GaussDB_opengauss_client_tools.zip>`__ the GaussDB driver package **GaussDB_opengauss_client_tools.zip**.

Prerequisites
-------------

-  You have downloaded the ODBC driver packages for Linux and Windows, as well as the header files (such as **sql.h** and **sqlext.h**) and library **libodbc.so** provided by unixODBC for application development. The header files and library can be obtained from the unixODBC-2.3.0 installation package.
-  Download the open-source unixODBC code file 2.3.0. For details, see https://sourceforge.net/projects/unixodbc/files/unixODBC/2.3.0/unixODBC-2.3.0.tar.gz/download.
-  Configure ODBC driver (psqlodbcw.so) in the data source. Before configuring the data source, configure the **odbc.ini** and **odbcinst.ini** files on the server. The two files are generated during the unixODBC compilation and installation, and are saved in the **/usr/local/etc** directory by default.

Procedure in a Linux Server
---------------------------

#. Install unixODBC. It does not matter if unixODBC of another version has been installed.

   Currently, unixODBC-2.2.1 is not supported. For example, to install unixODBC-2.3.0, run the commands below. It is installed in the **/usr/local** directory by default. The data source file is generated in the **/usr/local/etc** directory, and the library file is generated in the **/usr/local/lib** directory.

   .. code-block::

      tar zxvf unixODBC-2.3.0.tar.gz
      cd unixODBC-2.3.0
      # Modify the configure file and find LIB_VERSION.
      # Change its value to 1:0:0 to compile a *.so.1 dynamic library with the same dependency on psqlodbcw.so.
      vim configure

      ./configure --enable-gui=no # To perform the compilation on a Kunpeng server, add the configure parameter --build=aarch64-unknown-linux-gnu.
      make
      # Obtain require root permissions for installation.
      make install

#. Replace the GaussDB driver on the client.

   Decompress **GaussDB-Kernel-V**\ *xxx*\ **R**\ *xxx*\ **C**\ *xx*\ **-EULER-64bit-Odbc.tar.gz** to **/usr/local/lib**. **psqlodbcw.la** and **psqlodbcw.so** files are generated.

#. Configure the data source.

   a. Configure the ODBC driver file.

      Add the following content to the **/usr/local/etc/odbcinst.ini** file:

      .. code-block::

         [GaussMPP]
         Driver64=/usr/local/lib/psqlodbcw.so
         setup=/usr/local/lib/psqlodbcw.so

      For descriptions of the parameters in the **odbcinst.ini** file, see :ref:`Table 2 <en-us_topic_0000002124197037__en-us_topic_0059778464_td564f21e7c8e458bbd741b09896f5d91>`.

      .. _en-us_topic_0000002124197037__en-us_topic_0059778464_td564f21e7c8e458bbd741b09896f5d91:

      .. table:: **Table 2** Configuration parameters in odbcinst.ini

         +--------------+--------------------------------------------------------------------------------------+-------------------------------------+
         | Parameter    | Description                                                                          | Example                             |
         +==============+======================================================================================+=====================================+
         | [DriverName] | Driver name, corresponding to the driver name in the data source DSN.                | [DRIVER_N]                          |
         +--------------+--------------------------------------------------------------------------------------+-------------------------------------+
         | Driver64     | Path of the dynamic driver library.                                                  | Driver64=/xxx/odbc/lib/psqlodbcw.so |
         +--------------+--------------------------------------------------------------------------------------+-------------------------------------+
         | setup        | Driver installation path, which is the same as the dynamic library path in Driver64. | setup=/xxx/odbc/lib/psqlodbcw.so    |
         +--------------+--------------------------------------------------------------------------------------+-------------------------------------+

   b. Configure the data source file.

      Add the following content to the **/usr/local/etc/odbc.ini** file:

      .. code-block::

         [gaussdb]
         Driver=GaussMPP
         Servername=10.10.0.13 (database server IP)
         Database=postgres (database name)
         Username=omm (database username)
         Password= (database user password)
         Port=8000 (database listening port)
         Sslmode=allow

      For descriptions of the parameters in the **odbc.ini** file, see :ref:`Table 3 <en-us_topic_0000002124197037__en-us_topic_0059778464_t55845a6555f2454297b64ce47ad3d648>`.

      .. _en-us_topic_0000002124197037__en-us_topic_0059778464_t55845a6555f2454297b64ce47ad3d648:

      .. table:: **Table 3** Configuration parameters in odbc.ini

         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter             | Description                                                                                                                                      | Example                                                                                                                                                                                                                                            |
         +=======================+==================================================================================================================================================+====================================================================================================================================================================================================================================================+
         | [DSN]                 | Data source name.                                                                                                                                | [gaussdb]                                                                                                                                                                                                                                          |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Driver                | Driver name, corresponding to DriverName in **odbcinst.ini**.                                                                                    | Driver=DRIVER_N                                                                                                                                                                                                                                    |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Servername            | IP address of the server.                                                                                                                        | Servername=10.145.130.26                                                                                                                                                                                                                           |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Database              | Name of the database to connect to.                                                                                                              | Database=postgres                                                                                                                                                                                                                                  |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Username              | Database username.                                                                                                                               | Username=omm                                                                                                                                                                                                                                       |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Password              | Database user password.                                                                                                                          | Password=                                                                                                                                                                                                                                          |
         |                       |                                                                                                                                                  |                                                                                                                                                                                                                                                    |
         |                       |                                                                                                                                                  | .. note::                                                                                                                                                                                                                                          |
         |                       |                                                                                                                                                  |                                                                                                                                                                                                                                                    |
         |                       |                                                                                                                                                  |    The ODBC driver automatically clears password stored in the memory.                                                                                                                                                                             |
         |                       |                                                                                                                                                  |                                                                                                                                                                                                                                                    |
         |                       |                                                                                                                                                  |    However, if this parameter is configured, unixODBC will cache data source files, which may cause the password to be stored in the memory for a long time.                                                                                       |
         |                       |                                                                                                                                                  |                                                                                                                                                                                                                                                    |
         |                       |                                                                                                                                                  |    When you connect to an application, send your password through an API instead of writing it in a data source configuration file. After the connection has been established, immediately clear the memory segment where your password is stored. |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Port                  | Port number of the server.                                                                                                                       | Port=8000                                                                                                                                                                                                                                          |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Sslmode               | Whether to enable SSL.                                                                                                                           | Sslmode=allow                                                                                                                                                                                                                                      |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | UseServerSidePrepare  | Whether to enable the extended query protocol for the database.                                                                                  | UseServerSidePrepare=1                                                                                                                                                                                                                             |
         |                       |                                                                                                                                                  |                                                                                                                                                                                                                                                    |
         |                       | The value can be **0** or **1** (by default). The value **1** indicates that extended query protocol is enabled.                                 |                                                                                                                                                                                                                                                    |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | UseBatchProtocol      | Whether to enable the batch query protocol. If it is enabled, the DML performance can be improved. The value can be **0** or **1** (by default). | UseBatchProtocol=1                                                                                                                                                                                                                                 |
         |                       |                                                                                                                                                  |                                                                                                                                                                                                                                                    |
         |                       | If this parameter is set to **0**, the batch query protocol is disabled (mainly for communication with earlier database versions).               |                                                                                                                                                                                                                                                    |
         |                       |                                                                                                                                                  |                                                                                                                                                                                                                                                    |
         |                       | If this parameter is set to **1** and the **support_batch_bind** parameter is set to **on**, the batch query protocol is enabled.                |                                                                                                                                                                                                                                                    |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | ConnectionExtraInfo   | Whether to display the driver deployment path and process owner in the **connection_info** parameter.                                            | ConnectionExtraInfo=1                                                                                                                                                                                                                              |
         |                       |                                                                                                                                                  |                                                                                                                                                                                                                                                    |
         |                       |                                                                                                                                                  | .. note::                                                                                                                                                                                                                                          |
         |                       |                                                                                                                                                  |                                                                                                                                                                                                                                                    |
         |                       |                                                                                                                                                  |    The default value is **0**. If this parameter is set to **1**, the ODBC driver reports the driver deployment path and process owner to the database and records the information in the **connection_info** parameter.                           |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

      For values of the **sslmode** parameter, see the following table.

      .. table:: **Table 4** sslmode values and descriptions

         +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | sslmode     | Whether SSL Encryption Is Enabled | Description                                                                                                                                                                                                                                                                                                                                                  |
         +=============+===================================+==============================================================================================================================================================================================================================================================================================================================================================+
         | disable     | No                                | SSL connection is not used.                                                                                                                                                                                                                                                                                                                                  |
         +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | allow       | Maybe                             | SSL connection is used if require by the database server, but does not check the authenticity of the server.                                                                                                                                                                                                                                                 |
         +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | prefer      | Maybe                             | SSL connection is used as a preferred mode if supported by the database, but does not check the authenticity of the server.                                                                                                                                                                                                                                  |
         +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | require     | Yes                               | SSL connection must be used, but it only encrypts data and does not check the authenticity of the server.                                                                                                                                                                                                                                                    |
         +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | verify-ca   | Yes                               | SSL connection must be used, and it checks whether certificates are issued by a trusted CA.                                                                                                                                                                                                                                                                  |
         +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | verify-full | Yes                               | SSL connection must be used. In addition to the check scope specified by **verify-ca**, it checks whether the name of the host where the database is located is the same as that on the certificate. If they are different, modify the **/etc/hosts** file as user **root** and add the IP address and host name of the connected database node to the file. |
         +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Configure SSL mode.

   Declare the following environment variables and ensure that the permission for the **client.key\*** series files is set to **600**.

   .. code-block::

      export PGSSLCERT= "/YOUR/PATH/OF/client.crt" # Change the path to the absolute path of client.crt.
      export PGSSLKEY= "/YOUR/PATH/OF/client.key" # Change the path to the absolute path of client.key.
      Save the root certificate cacert.pem to the .postgresql directory in the home directory of the client user (if the directory does not exist, create it), rename the cacert.pem file as root.crt, and set the file permission to 600.

   Change the value of **sslmode** in the data source to **require**.

#. Configure environment variables.

   .. code-block::

      vim ~/.bashrc

   Add the following content to the configuration file:

   .. code-block::

      export LD_LIBRARY_PATH=/usr/local/lib/:$LD_LIBRARY_PATH
      export ODBCSYSINI=/usr/local/etc
      export ODBCINI=/usr/local/etc/odbc.ini

#. Make the modification take effect.

   .. code-block::

      source ~/.bashrc

#. Connect to the database.

   **isql -v** *GaussODBC*

   *GaussODBC*: data source name.

   -  If the following information is displayed, the configuration is correct and the connection succeeds.

      .. code-block::

         +---------------------------------------+
         | Connected!                            |
         |                                       |
         | sql-statement                         |
         | help [tablename]                      |
         | quit                                  |
         |                                       |
         +---------------------------------------+
         SQL>

   -  If error information is displayed, the configuration is incorrect.

Procedure in a Windows Server
-----------------------------

Configure the ODBC data source using the ODBC data source manager preinstalled in a Windows server.

#. Replace the GaussDB driver on the client.

   Decompress **GaussDB-Kernel-VxxxRxxxCxx-Windows-Odbc-X86.tar.gz** and install **psqlodbc.msi** (for 32-bit OS) or **psqlodbc_x64.msi** (for 64-bit OS) as required.

#. Open Driver Manager.

   Use the Driver Manager suitable for your OS to configure the data source. (Assume the Windows system drive is drive C.)

   -  If you develop 32-bit programs in the 64-bit OS, open the 32-bit Driver Manager at **C:\\Windows\\SysWOW64\\odbcad32.exe** after you install the 32-bit driver.

      Do not choose **Control Panel** > **Administrative Tools** > **Data Sources (ODBC)** directly.

      .. note::

         WoW64 is short for "Windows 32-bit on Windows 64-bit". **C:\\Windows\\SysWOW64\\** stores the 32-bit environment on a 64-bit system. **C:\\Windows\\System32\\** stores the environment consistent with the current OS. For technical details, see Windows technical documents.

   -  If you develop 64-bit programs in the 64-bit OS, open the 64-bit Driver Manager at **C:\\Windows\\System32\\odbcad32.exe** after you install the 64-bit driver.

      Do not choose **Control Panel** > **Administrative Tools** > **Data Sources (ODBC)** directly.

   -  In a 32-bit OS, open **C:\\Windows\\System32\\odbcad32.exe**.

      Alternatively, choose **Computer** > **Control Panel** > **Administrative Tools** > **Data Sources (ODBC)** directly.

#. Configure the data source.

   On the **User DSN** tab, click **Add**, and choose **PostgreSQL Unicode** for setup. (An identifier will be displayed for the 64-bit OS.)

   |image1|

   .. important::

      The entered username and password will be recorded in the Windows registry and you do not need to enter them again when connecting to the database next time. For security purposes, delete sensitive information before clicking **Save** and enter the required username and password when using ODBC APIs to connect to the database.

#. Configure SSL mode.

   Save the **client.crt**, **client.key**, **client.key.cipher**, and **client.key.rand** files in the **%APPDATA%\\postgresql** directory (which is manually created). Change **client** in the file names to **postgres**, for example, change **client.key** to **postgres.key**. Save the **cacert.pem** file to the **%APPDATA%\\postgresql** directory and change the file name to **root.crt**.

   .. note::

      The default value of **%APPDATA %** is **C:\\Users\\[username]\\AppData**. You can specify its value during installation.

   In addition, change the value of **sslmode** to **require**.

   .. table:: **Table 5** sslmode values and descriptions

      +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | sslmode     | Whether SSL Encryption Is Enabled | Description                                                                                                                                                                                                                                                        |
      +=============+===================================+====================================================================================================================================================================================================================================================================+
      | disable     | No                                | SSL connection is not used.                                                                                                                                                                                                                                        |
      +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | allow       | Maybe                             | SSL connection is used if require by the database server, but does not check the authenticity of the server.                                                                                                                                                       |
      +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | prefer      | Maybe                             | SSL connection is used as a preferred mode if supported by the database, but does not check the authenticity of the server.                                                                                                                                        |
      +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | require     | Yes                               | SSL connection must be used, but it only encrypts data and does not check the authenticity of the server.                                                                                                                                                          |
      +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | verify-ca   | Yes                               | SSL connection must be used, and it checks whether certificates are issued by a trusted CA. Currently, Windows ODBC does not support cert authentication.                                                                                                          |
      +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | verify-full | Yes                               | SSL connection must be used. In addition to the check scope specified by **verify-ca**, it checks whether the name of the host where the database is located is the same as that on the certificate. Currently, Windows ODBC does not support cert authentication. |
      +-------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **Test** to test the connection.

   -  If the following information is displayed, the configuration is correct and the connection succeeds.

      |image2|

   -  If error information is displayed, the configuration is incorrect. Check the configuration.

.. |image1| image:: /_static/images/en-us_image_0000002088678114.jpg
.. |image2| image:: /_static/images/en-us_image_0000002124197557.jpg
