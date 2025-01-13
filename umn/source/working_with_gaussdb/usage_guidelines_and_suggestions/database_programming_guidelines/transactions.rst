:original_name: opengauss_tips_0024.html

.. _opengauss_tips_0024:

Transactions
============

-  In GTM-free mode, cross-node transactions are not allowed.

   In GTM-free mode, if the executed SQL statement contains cross-node transactions, an error is reported.

   #. If the statement is split into multiple statements, the following error is reported:

      .. code-block::

         INSERT/UPDATE/DELETE/MERGE contains multiple remote queries under GTM-free modeUnsupport DML two phase commit under gtm free mode. modify your SQL to generate light-proxy or fast-query-shipping plan.

      In this case, you need to modify the statement to execute it on a single node.

   #. If the statement involves multiple nodes, the following error is reported:

      .. code-block::

         Your SQL needs more than one datanode to be involved in.

      You are advised to modify the statement so that it can be executed on a single node. If the statement needs to be executed on multiple nodes, add a hint, for example, **insert /*+ multinode \*/ into t values(3,3),(1,1);**

      It is recommended that **application_type** is set to **perfect_sharding_type** in the JDBC connection string in the development phase. In this way, errors are reported for all cross-node read and write SQL statements, prompting developers to optimize statements as soon as possible.

-  Large object operations do not support transactions.

   .. note::

      Large object operations include creating and deleting DATABASE, ANAYLIZE, and VACCUM jobs.

-  Do not combine multiple SQL statements into one statement when accessing the database through JDBC

   When multiple statements are combined into one statement that contains object operations, if the intermediate object operation fails, a new transaction is started to execute subsequent statements.

   Example: The following statement does not meet the requirements.

   .. code-block::

      Connection conn = ....
      try {
          Statement stmt = null;
          try {
              stmt = conn.createStatement();
              stmt.executeUpdate("CREATE TABLE t1 (a int); DROP TABLE t1");
          } finally {
              stmt.close();
          }
          conn.commit();
      } catch(Exception e) {
         conn.rollback();
      } finally {
         conn.close();
      }

   In the preceding statements, if **CREATE TABLE t1;** fails, a new transaction is started to execute **DROP TABLE t1;**. As a result, the execution fails. The statement needs to be split to two statements and sent respectively:

   .. code-block::

      Connection conn = ....
      try {
          Statement stmt = null;
          try {
              stmt = conn.createStatement();
              stmt.executeUpdate("CREATE TABLE t1 (a int)");
              stmt.executeUpdate("DROP TABLE t1");
          } finally {
              stmt.close();
          }
          conn.commit();
      } catch(Exception e) {
         conn.rollback();
      } finally {
         conn.close();
      }
