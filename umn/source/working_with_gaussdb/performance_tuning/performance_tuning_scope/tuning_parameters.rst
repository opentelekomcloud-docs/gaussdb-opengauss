:original_name: opengauss_Parameter_optimization.html

.. _opengauss_Parameter_optimization:

Tuning Parameters
=================

Database parameters are key configuration items in a database system. Improper parameter settings may adversely affect database performance. This section describes some important parameters for reference. For details about more parameters, see :ref:`Exporting Parameters <opengauss_01_0049>`.

For details on how to modify parameters on the console, see :ref:`Modifying Instance Parameters <opengauss_01_0042>`.

Query Parameters
----------------

-  **track_stmt_session_slot**

   Specifies the maximum number of full or slow SQL statements that can be cached in a session.

   Cached SQL statements are periodically written to the system catalog. If the number of full or slow SQL statements exceeds the value of this parameter, new statements will not be traced until the flush thread flushes the cached statements to the disk to reserve idle space. However, the statements can be executed.

-  **effective_cache_size**

   Specifies the size of the disk buffer available to the optimizer in a single query. When configuring the parameter, you should consider both shared buffers and kernel disk caches. The expected number of concurrent queries on different tables need to be considered because these queries will share the available space. This parameter has no effect on the size of shared memory. It is used only for estimation purposes and does not reserve kernel disk caches. The value is in the unit of disk page. Usually the size of each page is 8192 bytes.

   **Value range**: an integer. It is from **1** to **INT_MAX** and the unit is 8 KB.

   A value greater than the default one may enable index scanning, and a value less than the default one may enable sequence scanning.

-  **enable_stream_operator**

   Specifies the query planner' use of streams. When this parameter is set to **off**, a large number of logs indicating that the stream plans cannot be pushed down are recorded.

-  **log_min_duration_statement**

   Causes the duration of each completed statement to be logged if the statement ran for at least the specified number of milliseconds. This parameter helps you trace the query statements that need to be optimized. For clients using extended query protocol, durations of the Parse, Bind, and Execute steps are logged independently.

   The value **-1** disables logging statement durations. If this parameter is set to a small value, the load throughput may be affected.

Auditing Parameters
-------------------

-  **audit_system_object**

   Determines whether to audit the CREATE, DROP, and ALTER operations on database objects. Database objects include databases, users, schemas, and tables. By changing the value of this parameter, you can audit only the operations on the required database objects. It is suitable for the scenarios where a standby node is forcibly promoted to primary.

   If this parameter is incorrectly changed, DDL audit logs will be lost. To change this parameter, contact technical support.

Lock Management
---------------

-  **update_lockwait_timeout**

   Specifies the maximum wait time of a single lock when data in the same row is concurrently updated. If the wait time of a lock exceeds the value of this parameter, the system reports an error. The value **0** indicates no timeout. The default value is **2 min**.

Connection and Authentication
-----------------------------

-  **session_timeout**

   Specifies the time that a session can remain inactive before being timed out. This function is disabled when it is set to **0**.

-  **failed_login_attempts**

   Specifies the maximum number of failed password attempts. If the number of failed password attempts reaches the value, the account is automatically locked. When this parameter is set to **0**, the number of failed password attempts is not limited.

-  **password_effect_time**

   Specifies the validity period of an account password. The value **0** indicates that the function is disabled.

-  **password_lock_time**

   Determines how many days an account is locked.
