:original_name: opengauss_opti_0038.html

.. _opengauss_opti_0038:

Selecting a Storage Type
========================

During database design, some key factors about table design will greatly affect the subsequent query performance of the database. Table design affects data storage as well. Scientific table design reduces I/O operations and minimizes memory usage, improving the query performance.

Selecting a proper storage type for your service is the first step of table definition.

+-----------------------------------+------------------------------------------------------------------------------+
| Storage Type                      | Scenario                                                                     |
+===================================+==============================================================================+
| Row storage                       | Point query returned with a few records. This is a simple index-based query. |
|                                   |                                                                              |
|                                   | In this scenario, data is frequently inserted, deleted, and updated.         |
+-----------------------------------+------------------------------------------------------------------------------+
| Column storage                    | Statistics analysis query where data is grouped or joined frequently.        |
+-----------------------------------+------------------------------------------------------------------------------+
