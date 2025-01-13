:original_name: opengauss_tips_0025.html

.. _opengauss_tips_0025:

JDBC
====

-  A database must be specified for the JDBC instance. Once the instance is created, the database cannot be changed.

-  The length of a single SQL statement cannot exceed 2 GB. Considering the communications cost, it is recommended that the length of a single SQL statement cannot exceed 5,000 lines.

-  Prepare Execute cannot be used for DDL statements.

-  fetchsize can be used only when autocommit is disabled. Otherwise, fetchsize is invalid.

-  You are advised to use the default GUC parameters and need to avoid sending SET requests through JDBC to modify GUC parameters.

   .. note::

      For details, see :ref:`GUC Parameter Programming Guidelines <opengauss_tips_0015>`.

-  You are advised to use Prepare Execute to execute query statements to improve execution efficiency.

-  The time zone of the host where the JDBC client is located, the time zone of the host where the DB instance is located, and the time zone during instance configuration must be the same.

-  If a temporary table is created in a connection, you need to delete the temporary table before releasing the connection to the connection pool to avoid errors.

-  Set **prepareThreshold** to a proper value. If the query statement is fixed, set it to **1**.

-  You are advised to set the connection parameter **autobalance** to **true** to enable the CN load balancing function and set multiple CN connection addresses (separated by commas).

   Once **autobalance** is enabled, the JDBC driver attempts to allocate JDBC connections to different CNs.

   The purpose of setting multiple CN connection addresses is to prevent the JDBC driver from failing to obtain the CN list of the DB instance for the first time due to a CN fault.

   Once the CN list is obtained successfully for the first time, the system does not depend on the CN list specified in the connection parameters. Instead, the system obtains the latest CN list by connecting to a valid CN in the CN list obtained in real time at a specified interval.

-  Configure a proper JDBC connection timeout based on the upper-layer request timeout of the service to prevent the job from occupying database resources continuously.

   .. note::

      Timeout parameters include **loginTimeout**, **connectTimeout**, and **socketTimeout**.

      -  **loginTimeout**: integer type. This parameter indicates the waiting time for establishing the database connection, in seconds.
      -  **connectTimeout**: integer type. This parameter indicates the timeout duration for connecting to CNs. If the time taken to connect to a CN exceeds the value specified, the connection is disconnected. If the value is **0**, the timeout mechanism is disabled.
      -  **socketTimeout**: integer type. This parameter indicates the timeout duration for a socket read operation. If the time taken to read data from a CN exceeds the value specified, the connection is closed. If the value is **0**, the timeout mechanism is disabled.
      -  **cancelSignalTimeout**: integer type. Canceling messages may cause a block. This parameter controls **connectTimeout** and **socketTimeout** in a cancel message, in seconds. The default value is 10 seconds.
      -  **tcpKeepAlive**: Boolean type. This parameter is used to enable or disable TCP keepalive detection. The default value is **false**.

      The preceding parameters can be configured in the JDBC connection string or property connection attribute. For example:

      #. Configure the following information in the connection string:

         .. code-block::

            jdbc:postgresql://host:port/postgres?tcpKeepAlive=true

      2. Configure the following information in the property connection attribute:

         .. code-block::

            Properties info = new Properties();
            Info.setProperty("tcpKeepAlive", true);
