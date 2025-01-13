:original_name: opengauss_opti_0062.html

.. _opengauss_opti_0062:

Hint Errors, Conflicts, and Other Warnings
==========================================

Plan hints change an execution plan. You can run **EXPLAIN** to view the changes.

Hints containing errors are invalid and do not affect statement execution. The errors will be displayed in different ways based on statement types. Hint errors in an **EXPLAIN** statement are displayed as a warning on the interface. Hint errors in other statements will be recorded in debug1-level logs containing the **PLANHINT** keyword.

Hint error types are as follows:

-  Syntax errors

   An error will be reported if the syntax tree fails to be reduced. The No. of the row generating an error is displayed in the error details.

   For example, the hint keyword is incorrect, no table or only one table is specified in the **leading** or **join** hint, or no tables are specified in other hints. The parsing of a hint is terminated immediately after a syntax error is detected. Only the hints that have been parsed successfully are valid.

   For example:

   leading((t1 t2)) nestloop(t1) rows(t1 t2 #10)

   The syntax of **nestloop(t1)** is wrong and its parsing is terminated. Only **leading(t1 t2)** that has been successfully parsed before **nestloop(t1)** is valid.

-  Semantic errors

   -  An error will be reported if the specified tables do not exist, multiple tables are found based on the hint setting, or a table is used more than once in the **leading** or **join** hint.
   -  An error will be reported if the index specified in a scan hint does not exist.
   -  If multiple tables with the same name exist after a subquery is pulled up and some of them need to be hinted, add aliases for them to avoid name duplication.

-  Duplicated or conflicted hints

   If hint duplication or conflicts occur, only the first hint takes effect. A message will be displayed to describe the situation.

   -  Hint duplication specifies that a hint is used more than once in the same query, for example, **nestloop(t1 t2) nestloop(t1 t2)**.

   -  A hint conflict specifies that the functions of two hints with the same table list conflict with each other.

      For example, if **nestloop (t1 t2) hashjoin (t1 t2)** is used, **hashjoin (t1 t2)** becomes invalid. **nestloop(t1 t2)** does not conflict with **no mergejoin(t1 t2)**.

.. important::

   The table list in the **leading** hint is disassembled. For example, **leading ((t1 t2 t3))** will be disassembled as **leading((t1 t2)) leading(((t1 t2) t3))**, which will conflict with **leading((t2 t1))** (if any). In this case, the latter **leading(t2 t1)** becomes invalid. If two hints use duplicated table lists and only one of them has the specified outer/inner table, the one without a specified outer/inner table becomes invalid.

-  A hint becomes invalid after a sublink is pulled up.

   In this case, a message will be displayed. Generally, such invalidation occurs when a sublink contains multiple tables to be joined. After the sublink is pulled up, the tables will not be join members.

-  Unsupported column types

   -  Skew hints are specified to optimize redistribution. They will be invalid if their corresponding columns do not support redistribution.

-  Hints are not used.

   -  If **hashjoin** or **mergejoin** is specified for non-equivalent joins, it will not be used.
   -  If **indexscan** or **indexonlyscan** is specified for a table that does not have an index, it will not be used.
   -  If **indexscan hint** or **indexonlyscan** is specified for a full-table scan or for a scan whose filtering conditions are not configured on index columns, it will not be used.
   -  The specified **indexonlyscan** hint is used only when the output column contains only indexes.
   -  In equivalent joins, only the joins containing equivalence conditions are valid. Therefore, the **leading**, **join**, and **rows** hints specified for the joins without an equivalence condition will not be used. For example, **t1**, **t2**, and **t3** are to be joined, and the join between **t1** and **t3** does not contain an equivalence condition. In this case, **leading(t1 t3)** will not be used.
   -  To generate a streaming plan, if the distribution key of a table is the same as its join key, **redistribute** specified for this table will not be used. If the distribution key and join key are different for this table but the same for the other table in the join, **redistribute** specified for this table will be used but **broadcast** will not.
   -  If no sublink is pulled up, the specified **blockname** hint will not be used.
   -  Skew hints are not used possibly because:

      -  The plan does not require redistribution.
      -  The columns specified by hints contain distribution keys.
      -  Skew information specified in hints is incorrect or incomplete, for example, no value is specified for join optimization.
      -  Skew optimization is disabled by GUC parameters.
