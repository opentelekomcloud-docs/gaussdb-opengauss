:original_name: opengauss_tips_0020.html

.. _opengauss_tips_0020:

UPDATE
======

-  LIMIT cannot be directly used in the UPDATE statement. The WHERE condition must be used to specify the target row to be updated.

-  In GTM-free mode, cross-node transactions are not allowed. Therefore, when updating a data table distributed by hash, you must specify the equal condition in the WHERE condition for the distributed columns.

-  Updating multiple tables is not supported.

   Multi-table update is to update multiple tables in a single SQL statement.

-  The UPDATE statement must contain the WHERE clause to avoid full table scanning.

-  Do not use the updated column as the update source when the UPDATE clause updates multiple columns simultaneously.

   Multiple columns are updated at the same time, and the update sources are the same. The behavior varies depending on the database. To avoid compatibility issues, avoid the preceding operations at the service layer.

   Example:

   **UPDATE** table **SET** col1 = col2, col3 = col1 **WHERE** col1 = 1;

   In GaussDB, the value of **col3** is the original value of **col1**. In MySQL databases, the value of **col3** is the value of **col2** (because the value of **col2** is assigned to **col1**).

-  Do not use the ORDER BY or GROUP BY clause in the UPDATE statement to avoid unnecessary sorting.

-  If a table has a primary key or index, the WHERE condition must be used together with the primary key or index during update.
