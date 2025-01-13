:original_name: opengauss_tips_0013.html

.. _opengauss_tips_0013:

Index Design Guidelines
=======================

-  Use the recommended index type in database index practice.

   Recommended types must be used for index design. If you need to use prohibited, not recommended, or restricted index types, contact GaussDB database experts for evaluation.

   .. table:: **Table 1** Recommended database indexes

      +-----------------------------+--------------------------------------------------------------------------------------------------------+------------------------+
      | **Index Type**              | **Description**                                                                                        | **Recommended or Not** |
      +-----------------------------+--------------------------------------------------------------------------------------------------------+------------------------+
      | Primary key or unique index | Single-column or multi-column primary key or unique index                                              | Yes                    |
      +-----------------------------+--------------------------------------------------------------------------------------------------------+------------------------+
      | Expression index            | An index column is a function or scalar expression calculated from one or multiple columns in a table. | No (Restricted)        |
      +-----------------------------+--------------------------------------------------------------------------------------------------------+------------------------+

-  For a distributed hash table, the primary key and unique index must contain distribution keys.

-  Properly design composite indexes to avoid redundancy.

   For example, if an index has been created for **(a, b, c)**, you shall not create an index for **(a)**, **(b)**, **(c)**, **(a, b)**, or **(b, c)** independently.

   If only the filtering condition of the **a** column is used for a query, composite indexes are used for the query as well.

-  Do not create multiple unique indexes for a single table.

   Maintaining multiple unique indexes at the same time generates more overheads than maintaining a multi-column unique index. If multiple unique indexes are equivalent to a multi-column unique index in service logic, a multi-column unique index shall be preferred.

-  A composite index contains up to 5 columns.

-  The total length of the combined character string of a composite index cannot exceed 200.

-  Index (including single-column and composite indexes) columns must be NOT NULL.

-  The efficiency of maintaining indexes created for the same column is different. Columns of the number type are better than those of the character type and other data types. Therefore, it is recommended that columns such as IDs and time for creating indexes be stored as data of the number type.

-  You are advised to create an index on the associated column.

   HASH JOIN is supported, whereas NESTLOOP JOIN may be used for join operations if the rescan cost is low (for example, the internal table is small). If the NESTLOOP JOIN plan can be viewed by executing EXPLAIN, you can create indexes on joined columns to improve the efficiency of NESTLOOP JOIN.
