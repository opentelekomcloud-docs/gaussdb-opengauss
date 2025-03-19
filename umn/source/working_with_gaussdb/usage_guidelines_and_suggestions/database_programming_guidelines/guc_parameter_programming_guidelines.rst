:original_name: opengauss_tips_0015.html

.. _opengauss_tips_0015:

GUC Parameter Programming Guidelines
====================================

Clients (such as JDBC) should use default (global) parameters for queries and cannot use session-level GUC parameters.

When you change GUC parameters through ODBC or JDBC, the GUC parameters take effect only in the current connection. Especially in the connection pool scenario, problems may occur and are difficult to be located.

If the GUC parameters must be configured in a connection, you must execute the following statements before releasing the connection to the connection pool:

**SET SESSION AUTHORIZATION DEFAULT;**

**RESET ALL;**

You need to clear the connection status.
