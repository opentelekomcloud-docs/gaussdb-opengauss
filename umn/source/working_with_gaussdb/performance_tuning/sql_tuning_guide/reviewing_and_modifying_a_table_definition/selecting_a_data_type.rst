:original_name: opengauss_opti_0043.html

.. _opengauss_opti_0043:

Selecting a Data Type
=====================

Use the following principles to obtain efficient data types:

#. **Selecting data types that facilitate data calculation**

   Generally, the calculation of integers (including common comparison calculations such as =, >, <, >=, <=, and !=, as well as GROUP BY) is more efficient than that of strings and floating point numbers. For example, if you need to perform a point query on a column-store table whose numeric column is used as a filter criterion, the query will take over 10s. If you change the data type from **NUMERIC** to **INT**, the query will be reduced to about 1.8s.

#. **Selecting data types with a short length**

   Data types with short length reduce both the data file size and the memory used for computing, improving the I/O and computing performance. For example, use **SMALLINT** instead of **INT**, and **INT** instead of **BIGINT**.

#. **Using the same data type for a join**

   You are advised to use the same data type for a join. To join columns with different data types, the database needs to convert them to the same type, leading to additional performance overheads.
