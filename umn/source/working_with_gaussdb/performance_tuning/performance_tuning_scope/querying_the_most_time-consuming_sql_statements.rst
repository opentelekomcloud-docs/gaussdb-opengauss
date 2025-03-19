:original_name: opengauss_opti_0027.html

.. _opengauss_opti_0027:

Querying the Most Time-Consuming SQL Statements
===============================================

This section describes how to identify SQL statements whose execution takes a long time, leading to poor system performance.

Procedure
---------

#. Use DAS or gsql to connect to an instance.

#. Query the statements that are run for a long time in the system.

   .. code-block::

      SELECT current_timestamp - query_start AS runtime, datname, usename, query FROM pg_stat_activity where state != 'idle' ORDER BY 1 desc;

   The command output lists the query statements in descending order by their execution duration length. The first record is the query statement that takes the longest time for execution. There are SQL statements invoked by the system and SQL statements run by users. Find the statements that were run by users and took a long time.

   Alternatively, you can set **current_timestamp - query_start** to be greater than a threshold to identify query statements that are executed for a duration longer than this threshold.

   .. code-block::

      SELECT query FROM pg_stat_activity WHERE current_timestamp - query_start > interval '1 days';

#. Set the parameter **track_activities** to **on**.

   .. code-block::

      SET track_activities = on;

   The database system collects the running information about active queries only if the parameter is set to **on**.

#. View the query statements that are executing.

   The **pg_stat_activity** view is used as an example.

   .. code-block::

      SELECT datname, usename, state FROM pg_stat_activity;
       datname  | usename | state  |
      ----------+---------+--------+
       postgres |   omm   | idle   |
       postgres |   omm   | active |
      (2 rows)

   If the state field is idle, the connection is idle and waits for a user to enter a command.

   To identify only active query statements, run the following command:

   .. code-block::

      SELECT datname, usename, state FROM pg_stat_activity WHERE state != 'idle';

#. Analyze the status of the query statements that were executed for a long time.

   -  If a query statement is in the normal state, wait until its execution is complete.

   -  If a query statement is blocked, run the following command to find the statement:

      .. code-block::

         SELECT datname, usename, state, query FROM pg_stat_activity WHERE waiting = true;

      The command output lists a query statement in the block state. The lock resource requested by this query statement is occupied by another session, so this query statement is waiting for the session to release the lock resource.

   Only when the query is blocked by internal lock resources, **waiting** is **true**. In most cases, blocks happen when query statements are waiting for lock resources to be released. However, query statements may be blocked due to write operations and timers. Such blocked queries are not displayed in the **pg_stat_activity** view.
