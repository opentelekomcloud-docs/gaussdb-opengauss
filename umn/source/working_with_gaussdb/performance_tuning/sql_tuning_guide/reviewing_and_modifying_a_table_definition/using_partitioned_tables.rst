:original_name: opengauss_opti_0042.html

.. _opengauss_opti_0042:

Using Partitioned Tables
========================

A partitioned table is a logical table that is divided into several physical partitions for storage based on a specific plan. Data is stored on the physical partitions, instead of the partitioned table. A partitioned table has the following advantages over an ordinary table:

#. High query performance: The system queries only the relevant partitions rather than the entire table, improving the query efficiency.
#. High availability: A faulty partition does not affect data availability in other partitions.
#. Easy maintenance: If a certain partition in the table is faulty, only this partition rather than the entire table needs to be repaired.

GaussDB supports range partitioned tables.

In range partitioned tables, data is mapped to each partition based on the range. The range is determined by the partition key specified when the partitioned table is created. The partition key is usually a date. For example, sales data is partitioned by month.
