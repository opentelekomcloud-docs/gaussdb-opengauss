:original_name: opengauss_opti_0028.html

.. _opengauss_opti_0028:

Checking Blocked Statements
===========================

During database running, if query statements are blocked and executed for an excessively long time, you can forcibly terminate the faulty sessions.

Procedure
---------

#. Use DAS or gsql to connect to an instance.

#. View blocked query statements and information about the tables and schemas that block the query statements.

   .. code-block::

      SELECT w.query as waiting_query,
      w.pid as w_pid,
      w.usename as w_user,
      l.query as locking_query,
      l.pid as l_pid,
      l.usename as l_user,
      t.schemaname || '.' || t.relname as tablename
      from pg_stat_activity w join pg_locks l1 on w.pid = l1.pid
      and not l1.granted join pg_locks l2 on l1.relation = l2.relation
      and l2.granted join pg_stat_activity l on l2.pid = l.pid join pg_stat_user_tables t on l1.relation = t.relid
      where w.waiting;

   The command output includes a thread ID, user information, query status, and table or schema that caused the block.

#. Terminate the required session:

   .. code-block::

      SELECT PG_TERMINATE_BACKEND(139834762094352);

   where **139834762094352** is the thread ID.

   If information similar to the following is displayed, the session is successfully terminated.

   .. code-block::

       PG_TERMINATE_BACKEND
      ----------------------
       t
      (1 row)

   If information similar to the following is displayed, the session is being terminated.

   .. code-block::

      FATAL:  terminating connection due to administrator command
      FATAL:  terminating connection due to administrator command

   .. note::

      -  When an initial user runs the **PG_TERMINATE_BACKEND** function to terminate the background threads of active sessions, the gsql client is reconnected automatically rather than be logged out. The message "The connection to the server was lost. Attempting reset: Succeeded." is returned. If non-initial users do this operation, the message "The connection to the server was lost. Attempting reset: Failed." is returned. This is because only initial users can log in to the system in password-free mode.
      -  When the **PG_TERMINATE_BACKEND** function is used to terminate the background threads of idle sessions and the thread pool is opened, idle sessions do not have thread IDs and cannot be terminated. In non-thread pool mode, terminated sessions are not automatically reconnected.
