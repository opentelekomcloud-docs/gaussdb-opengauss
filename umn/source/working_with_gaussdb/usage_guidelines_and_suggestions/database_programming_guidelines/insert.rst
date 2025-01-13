:original_name: opengauss_tips_0019.html

.. _opengauss_tips_0019:

INSERT
======

-  INSERT ON DUPLICATE KEY UPDATE does not support UPDATE on columns with primary keys or unique constraints.

   The semantics of INSERT ON DUPLICATE KEY UPDATE is to update the rows that have unique constraint conflicts. In this process, the value of the constraint should not be updated.

-  INSERT ON DUPLICATE KEY UPDATE is used to insert multiple data records, and the primary key or unique constraint conflict is not allowed between the data records.

   However, for MySQL, the conflict is allowed, for example, running the following statement:

   **INSERT INTO t1 VALUES**\ (1, 1), (1, 2) **ON DUPLICATE KEY UPDATE** col2 = VALUES(col2);

   According to SQL standard, the inserted lines cannot be modified at the same time in the same SQL command. Otherwise, the result is unpredictable. In the preceding example, if **col1** is the primary key, a record whose primary key is 1 is inserted into the same command and the inserted record is modified. Because transaction serialization cannot determine which operation is performed first, the result may be unstable.

   If the inserted data conflicts, it is difficult to determine which record is used for update in the distributed scenario.

-  Do not execute INSERT ON DUPLICATE KEY UPDATE on a table that has multiple unique constraints.

   .. note::

      A table may have multiple unique indexes, or both primary keys and unique indexes.

      When multiple unique constraints exist, the system checks all the unique constraints by default. If any constraint conflicts, the system updates the conflict row. That is, multiple records may be updated, which does not meet the service expectation. More specific conditions for inserting and updating data should be added.

-  You are advised to use INSERT INTO TABLE1 VALUES ``(),(),()`` for batch insertion because it is more efficient than executing multiple INSERT INTO VALUES() statements.

   .. note::

      Currently, whether multiple values belong to the same shard cannot be identified. Therefore, you need to use **/*+ multinode \*/** to run the statement.

      The keyword must be **VALUES** regardless of whether a single record is inserted or multiple records are inserted.

      INSERT INTO mytable VALUE() used in MySQL is not supported.
