:original_name: opengauss_tips_0011.html

.. _opengauss_tips_0011:

Table Design Guidelines
=======================

-  You must specify table distribution (DISTRIBUTE BY). The principles for selecting a table distribution policy are as follows:

   Two table distribution policies are provided: REPLICATION and HASH. REPLICATION retains a complete data table on each node. HASH distributes table data to multiple nodes based on the provided distribution key values.

   -  If the number of data records in a system configuration table or data dictionary table is less than 20 million and the insert and update operations are of low frequency, REPLICATION distribution policy is recommended.

      .. important::

         Exercise caution when using the REPLICATION distribution. This distribution table may cause space expansion and DML performance deterioration.

   -  For tables with a large amount of data and high update frequency, data must be sharded and hash distribution policy is required. It is recommended that the distribution key be one or more fields in the primary key.

-  Properly design distribution keys to facilitate query development and ensure even data storage, avoiding data skew and read hotspots.

   A distribution key is important for a hash table. An improper distribution key may cause data skew. As a result, the I/O load is heavy on several DNs, affecting the overall query performance. After you select a distribution policy for a hash table, check data skew to ensure that data is evenly distributed.

   #. Use columns with discrete values as distribution keys so that data can be evenly distributed to each DN.
   #. If the preceding requirement is met, it is not recommended that columns with constant filtering be used as distribution keys. Otherwise, all query tasks will be distributed to a unique and fixed DN.
   #. If the preceding two requirements are met, select the JOIN condition as the distribution key. In this way, related data of the JOIN task is distributed on the same DN, reducing the cost of data flow between DNs.

      .. note::

         Avoid data shuffle. To shuffle data is to physically transfer it from one node to another. This unnecessarily occupies many network resources. To reduce network pressure, locally process data, and improve instance performance and concurrency, you can minimize data shuffling using proper association and grouping conditions.

   #. According to the database specifications, the primary key of the HASH distribution table must contain the distribution column. Therefore, you can select the primary key of the table as the distribution key.

      .. table:: **Table 1** **Common distribution keys and effects**

         +--------------------------------------------------------------------------------------------------------------------------+---------------------------+
         | Distribution Key Value                                                                                                   | Event Distribution or Not |
         +==========================================================================================================================+===========================+
         | User ID. Many users exist in an application.                                                                             | Yes                       |
         +--------------------------------------------------------------------------------------------------------------------------+---------------------------+
         | Status code. Only a little status code is available.                                                                     | No                        |
         +--------------------------------------------------------------------------------------------------------------------------+---------------------------+
         | Date when a project is created. The value is rounded off to the latest time segment (for example, day, hour, or minute). | No                        |
         +--------------------------------------------------------------------------------------------------------------------------+---------------------------+
         | Device ID. Each device accesses data at a relatively similar interval.                                                   | Yes                       |
         +--------------------------------------------------------------------------------------------------------------------------+---------------------------+

-  The length of the column used by the distribution key cannot exceed 128 characters. If the length is too long, the computing overhead is high.

-  Once a distribution key is inserted, it cannot be updated unless you delete the key and insert it again.

-  Views cannot be nested.

   On the one hand, if a wildcard is used when a view is written, an error occurs in the view when a column is added to or deleted from the called view.

   On the other hand, view nesting may be inefficient because indexes cannot be used. You are advised to use base tables with indexes instead of performing join operations on views.

-  It is recommended that the number of distribution key columns cannot exceed 3. Too many columns will cause high computing overhead.

-  Avoid collation operations in a view definition.

   ORDER BY is invalid in the top-level view. If you must collate the output data, use ORDER BY in a called view.
