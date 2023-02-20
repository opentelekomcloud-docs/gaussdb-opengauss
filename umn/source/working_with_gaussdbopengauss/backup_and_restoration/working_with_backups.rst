:original_name: opengauss_01_0063.html

.. _opengauss_01_0063:

Working with Backups
====================

GaussDB(openGauss) supports backups and restorations to ensure data reliability.

Functions
---------

Although GaussDB(openGauss) supports high availability, if a database or table is maliciously or mistakenly deleted, data on the standby DB instance is also deleted. In this case, you can only restore the deleted data from backups.

Automated Backups
-----------------

Backups are automatically created during the backup period of a GaussDB(openGauss) DB instance. GaussDB(openGauss) saves automated backups based on the retention period you have specified. You can restore data from backups to any time point within the backup retention period. An automatic backup is triggered after CNs or shards are added.

Manual Backups
--------------

Manual backups are user-initiated full backups of DB instances. They are retained until you delete them manually.

Full Backups
------------

Full backup is to back up all selected data, even if no data is updated since the last backup.

Incremental Backups
-------------------

Incremental backups refer to differential backups. By default, GaussDB(openGauss) automatically backs up the data updated since the last automated backup every 30 minutes.
