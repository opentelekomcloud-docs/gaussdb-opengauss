:original_name: opengauss_tips_0021.html

.. _opengauss_tips_0021:

DELETE
======

-  LIMIT cannot be used in the DELETE statement. The WHERE condition should be used to specify the target row to be deleted.

-  In GMT-free mode, cross-node transactions are not allowed. Therefore, when deleting a data table distributed by hash, you must specify the equal condition in the WHERE condition for the distributed columns.

-  Deleting multiple tables is not supported.

   Multi-table deletion indicates that multiple tables are deleted in a single SQL statement.

-  The DELETE statement must contain the WHERE clause to avoid full table scanning.

-  Do not use the ORDER BY or GROUP BY clause in the DELETE statement to avoid unnecessary sorting.

-  Use TRUNCATE instead of DELETE to clear a table.

   TRUNCATE creates a new physical file and physically deletes the original file when the transaction ends to clear the disk space. However, the DELETE statement marks data in the table and does not clear the disk space until the VACCUUM FULL phase.

-  If a DELETE statement is executed on a table that has a primary key or index, the WHERE condition must be used together with the primary key or index to improve execution efficiency.
