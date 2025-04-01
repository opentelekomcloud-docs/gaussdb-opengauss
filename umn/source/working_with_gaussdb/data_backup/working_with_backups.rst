:original_name: opengauss_01_0063.html

.. _opengauss_01_0063:

Working with Backups
====================

You can back up your GaussDB instances to ensure data reliability. Currently, backups are stored in an unencrypted form.

Functions
---------

Although GaussDB supports high availability, if a database or table is maliciously or mistakenly deleted, data on the standby nodes is also deleted. In this case, you can only restore the deleted data from backups.

Full Backup
-----------

A full backup involves all data of a database at the backup point in time. The time required for full backup is long (in direct proportion to the total data volume of the database). You can use a full backup to restore data of a complete database. A full backup backs up all data even if the data has not changed since the last backup.

Differential Backup
-------------------

A differential backup involves only incremental data modified after a specified time point. It takes less time than a full backup in direct proportion to how much data has changed (The total data volume is irrelevant). However, a differential backup cannot be used to restore all of the data of a database. By default, the system automatically backs up updated data every 30 minutes since the last automated backup. The backup period can be changed from 15 minutes to 1,440 minutes.

Automated Backup
----------------

Backups are automatically created during the backup time window of a GaussDB instance. GaussDB saves automated backups based on a retention period you specify. An automated backup is triggered after CNs or shards are added.

Manual Backup
-------------

Manual backups are user-initiated full backups of instances. They are retained until you delete them manually.
