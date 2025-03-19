:original_name: opengauss_tips_0023.html

.. _opengauss_tips_0023:

Subqueries
==========

-  Duplicate subquery statements are not allowed in an SQL statement.

-  Do not use scalar queries.

   A scalar subquery is a subquery whose result is one value and whose condition expression uses an equal operator.

   Example: non-standard statement

   **SELECT \* FROM t1 WHERE id = (SELECT id FROM t2 LIMIT 1));**

   You are advised to split the preceding statement into two SQL statements and execute the subquery first.

-  Do not use subqueries in the SELECT target columns. Otherwise, the plan cannot be pushed down, affecting the execution performance.

-  It is recommended that the nested subqueries cannot exceed two layers.

   Subqueries cause temporary table overhead. Therefore, complex queries must be optimized based on service logic.
