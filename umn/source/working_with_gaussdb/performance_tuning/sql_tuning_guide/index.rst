:original_name: opengauss_opti_0029.html

.. _opengauss_opti_0029:

SQL Tuning Guide
================

The aim of SQL tuning is to maximize the utilization of resources, including CPU, memory, disk I/O, and network I/O. To maximize resource utilization, you need to run SQL statements as efficiently as possible to achieve the highest performance at a lower cost. For example, when performing a typical point query, you can use a seq scan and a filter (that is, read every tuple and point query conditions for match). You can also use an index scan, which can be implemented at a lower cost but achieve the same effect. You can determine a proper instance deployment solution and table definition based on hardware resources and customer service characteristics. This is the basis of meeting performance requirements.

-  :ref:`Query Execution Process <opengauss_opti_0030>`
-  :ref:`Introduction to the SQL Execution Plan <opengauss_opti_0031>`
-  :ref:`Tuning Process <opengauss_opti_0034>`
-  :ref:`Updating Statistics <opengauss_opti_0035>`
-  :ref:`Reviewing and Modifying a Table Definition <opengauss_opti_0036>`
-  :ref:`Typical SQL Tuning Advantages <opengauss_opti_0044>`
-  :ref:`Experience in Rewriting SQL Statements <opengauss_opti_0051>`
-  :ref:`Resetting Key Parameters During SQL Tuning <opengauss_opti_0052>`
-  :ref:`Hint-based Tuning <opengauss_opti_0053>`
-  :ref:`Checking the Implicit Conversion Performance <opengauss_opti_0064>`

.. toctree::
   :maxdepth: 1
   :hidden: 

   query_execution_process
   introduction_to_the_sql_execution_plan/index
   tuning_process
   updating_statistics
   reviewing_and_modifying_a_table_definition/index
   typical_sql_tuning_advantages/index
   experience_in_rewriting_sql_statements
   resetting_key_parameters_during_sql_tuning
   hint-based_tuning/index
   checking_the_implicit_conversion_performance
