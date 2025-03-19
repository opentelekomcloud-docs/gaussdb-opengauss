:original_name: opengauss_tips_0017.html

.. _opengauss_tips_0017:

WHERE
=====

-  During table query, the WHERE condition must contain all partitioning keys. Otherwise, the query will be performed on multiple nodes, affecting the system concurrency and performance.

-  Do not compare table fields with the same WHERE condition.

   For example, the following statement does not meet specifications:

   **SELECT** \* **FROM** t1 **WHERE** col1 = col1;

   It should be changed into:

   **SELECT** \* **FROM** t1 **WHERE** col1 IS NOT NULL;

-  Do not include implicit data type conversion in WHERE conditions.

   After implicit conversion is performed in the database, the created index may fail to be used and reduce the database performance.

   You are advised to enable the GUC parameter **check_implicit_conversions** and disable **enable_fast_query_shipping** during development to check whether query statements contain implicit data types that may affect performance.

   **SET** enable_fast_query_shipping = off;

   **SET** check_implicit_conversions = true;

   Checking implicit data type conversion requires extra overhead. After the query statement is developed, you need to disable the **check_implicit_conversions** parameter and reset **enable_fast_query_shipping**.

   **Example:**

   The following statements do not meet specifications:

   The **phonenumber** field in the **t_tablename** table is of the VARCHAR type instead of the numeric type. When the following statements use **phonenumber** to filter conditions, the optimizer implicitly converts **phonenumber** to the bigint type.

   **SELECT** column1

   **INTO** i_l_variable1

   **FROM** t_tablename

   **WHERE** phonenumber = 13512345678;

   This will lead to the following consequences:

   #. DNs cannot be tailored. The plan is delivered to all DNs for execution.
   #. Index Scan cannot be used to scan data in the plan.

   The **phonenumber** field in the **t_tablename** table should be changed to the VARCHAR type.

   **SELECT** column1

   **INTO** i_l_variable1

   **FROM** t_tablename

   **WHERE** phonenumber = '13512345678';

-  Do not use expressions or functions in WHERE condition fields.

   When expressions or functions are used in condition fields, indexes become invalid, and each row of data is calculated, consuming unnecessary performance. Non-constant expressions cannot be converted to constant expressions in the preprocessing phase. Therefore, they cannot be used for pruning and the query statement scans all data.

   Example:

   The following statements do not meet specifications:

   **SELECT** income **FROM** table **WHERE** abs(income) > ?;

   **SELECT** income **FROM** table **WHERE** income \* 10 > ?;

   **SELECT** create_time

   **FROM** table

   **WHERE** date_format(create_time, '%Y%m%d %H:%i:%s') = '20090101 00:00:0';

   The statements should be changed into:

   **SELECT** income **FROM** table **WHERE** income > ? **OR** income < (-1) \* ?;

   **SELECT** income **FROM** table **WHERE** income > ?/10;

   **SELECT** create_time

   **FROM** table

   **WHERE** create_time = str_to_date('20090101 00:00:0', '%Y%m%d %H:%i:%s');

-  Do not use comparison operator (!=) when comparing with NULL in query conditions. Instead, use IS NULL or IS NOT NULL.

   Do not write **expression=NULL** or **expression !=NULL**, because **NULL** represents an unknown value, and these expressions cannot determine whether two unknown values are equal.

-  Do not use comparison operator (!=) in the index fields of the query condition to avoid invalid indexes.

-  In WHERE clauses, the filtering conditions should be sorted. The condition where the number of filtered records is small is listed at the beginning.

-  Filtering conditions in WHERE clauses should comply with unilateral rules. When the field name is placed on one side of a comparison operator, the optimizer automatically performs pruning optimization in some scenarios. Filtering conditions in a WHERE clause will be displayed in **col op expression** format, where **col** indicates a table column, **op** indicates a comparison operator, such as = and >, and **expression** indicates an expression that does not contain a column name.

   Example:

   The following statement is not recommended. Filter data based on the time column.

   **SELECT id, from_image_id, from_person_id, from_video_id FROM face_data WHERE** **current_timestamp(6) - time < '1 days'::interval;**

   The statement should be changed into:

   **SELECT id, from_image_id, from_person_id, from_video_id FROM face_data where time >\ current_timestamp(6) - '1 days'::interval;**

-  Do not compare the index fields of query condition with NULL (IS NULL and IS NOT NULL).

-  Do not use NOT in the index fields of query conditions.

-  Do not use NOT IN in the index fields of query conditions.

-  Do not place % at the beginning of the LIKE statement for fuzzy query.

   If % is placed at the beginning of the LIKE statement, the index cannot be used and the entire table will be scanned.

-  It is recommended the number of IN clauses in the WHERE statement be less than or equal to 500.

   .. note::

      During the query, the values of all the clauses will be compared to check whether they are equal, which increases the overhead.

      If the included values are relatively fixed, you need to create a REPLICATION table, write the clause data into the table, and then use INNER JOIN to implement the inclusion query.

-  If the IN clause in the WHERE condition is not a constant but a column in the table, you are advised to change it to a subquery.

   .. note::

      In this case, it is actually an unequal JOIN, which is executed through the nestloop plan. If the table is too large, the execution efficiency is low. You are advised to change the query to a subquery of the JOIN type.

      Example

      The following statements do not meet specifications:

      **SELECT col1, COALESCE(max(col2 - 1), 0)**

      **FROM t1, t2**

      **WHERE t1.col1 = ANY(VALUES(id1), (id2))**

      **GROUP BY col1;**

      The statements should be changed into:

      **SELECT** col1, **COALESCE**\ (max(tmp), 0) **FROM**

      (

      (

      **SELECT** col1, (col2-1) **AS** tmp

      **FROM** t1, t2

      **WHERE** t1.col1 = t2.id1 **AND** t1.col1 != t2.id2

      ) **UNION ALL** (

      **SELECT** col1, (col2-1) **AS** tmp

      **FROM** t1, t2

      **WHERE** t1.col1 = t2.id2

      )

      ) **GROUP BY** col1;

-  Use equal operators, instead of not equal operators.

   If a not equal operator (IN, BETWEEN, <, <=, >, or >=) is used in the WHERE condition, no index can be used in the following conditions because two range conditions cannot be used at the same time.
