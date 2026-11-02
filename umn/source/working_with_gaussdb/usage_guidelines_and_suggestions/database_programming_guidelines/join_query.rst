:original_name: opengauss_tips_0022.html

.. _opengauss_tips_0022:

Join Query
==========

-  The nesting depth of multi-table association must be less than 8.

   If the association nesting is too deep, slow SQL statements may be generated. You need to optimize the association nesting at the service layer.

-  When tables are joined for query, the join condition (ON) of each table must be specified to avoid Cartesian product.

   For example, in MySQL, JOIN is equivalent to CROSS JOIN and INNER JOIN. However, in the SQL standards, JOIN is equivalent to INNER JOIN only and must be used together with the ON condition.

   Negative examples: The following query statements return a Cartesian product:

   **SELECT \* FROM TABLE1 A , TABLE2 B;**

   **SELECT \* FROM TABLE1 A JOIN TABLE2 B ON 1=1;**

-  The JOIN mode must be specified based on the SQL standards during association. Do not use the JOIN keyword directly. Instead, use CROSS JOIN, INNER JOIN, LEFT JOIN, or RIGHT JOIN.

-  When multiple tables are joined for query, aliases shall be added to the tables to ensure that the statement logic is clear and easy to maintain.

-  Different columns have different comparison overheads. You are advised to use a field type with high efficiency for join columns.

   The comparison efficiency of the numeric type is much higher than that of the string type.

   The comparison efficiency of integers is much higher than that of numeric and floating-point types.

-  The join columns must be of the same data type to avoid the impact of implicit type conversion on the execution efficiency.

-  You are advised use table association, instead of nested subqueries, because subqueries will generate temporary tables and greatly affect SQL performance.

-  If a large number of NULL values exist in the join columns, you are advised to add the IS NOT NULL condition to the WHERE condition to improve the execution efficiency.
