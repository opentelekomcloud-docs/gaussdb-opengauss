:original_name: opengauss_opti_0002.html

.. _opengauss_opti_0002:

Development Guidelines
======================

If the connection pool mechanism is used during application development, comply with the following guidelines before you return the connection to the connection pool:

-  If GUC parameters are configured in the connection, run **SET SESSION AUTHORIZATION DEFAULT;RESET ALL;** to clear the connection status.
-  If a temporary table is used, delete it.

If you do not do so, the connection status in the connection pool will remain, affecting subsequent operations using the connection pool.
