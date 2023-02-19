:original_name: opengauss_01_0064.html

.. _opengauss_01_0064:

Configuring an Automated Backup Policy
======================================

Scenarios
---------

When you create a GaussDB(openGauss) DB instance, an automated backup policy is enabled by default. After a GaussDB(openGauss) DB instance is created, you can modify the automated backup policy as needed. GaussDB(openGauss) backs up data based on the automated backup policy you specified.

GaussDB(openGauss) backs up data at the DB instance level. If a database is faulty or data is damaged, you can restore it from backups to ensure data reliability. Backups are saved as packages in OBS buckets to ensure data confidentiality and durability. Since backing up data affects the database read and write performance, you are advised to enable automated backups during off-peak hours.

The default automated backup policy of GaussDB(openGauss) is as follows:

-  Retention period: 7 days
-  Time window: An hour within 24 hours, such as 01:00-02:00 or 12:00-13:00. The backup time is in UTC format. If the DST or standard time is switched, the backup time segment changes with the time zone.
-  Backup cycle: Each day of the week.
-  Incremental backup policy: Backup files are saved every 30 minutes by default.

Modifying an Automated Backup Policy
------------------------------------

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and a project.
#. Click **Service List**. Under **Database**, click **GaussDB**.
#. In the navigation pane on the left, choose **GaussDB(openGauss)** > **Instance Management**.
#. On the **Instance Management** page, click the target DB instance.
#. On the **Backups & Restorations** page, click **Modify Backup Policy**.

   -  Configuring a full backup policy

      -  **Retention Period** refers to the number of days that your automated backups can be retained. Increasing the retention period will improve data reliability.

         .. note::

            -  If you shorten the retention period, the new backup policy takes effect for all backup files. The backup files that have expired will be deleted.
            -  The retention period ranges from 1 to 732 days. To extend the retention period, contact customer service. Automated backups can be retained for up to 2562 days. The time window is one hour. You are advised to select an off-peak time window for full backups. By default, each day of the week is selected for **Backup Cycle**. You can change the backup cycle. At least one day must be selected.
            -  A full backup is immediately triggered after a DB instance is created. Then, a full backup or an incremental backup is performed based on the time window and backup cycle you specified. We recommend that you set the automated backup time window to off-peak hours.

      -  Select the backup cycle as required. At least one day must be selected.

   -  Configuring an incremental backup policy

      You need to select the backup cycle for performing an incremental backup. The 30 minutes is used by default.

#. Click **OK**.

.. |image1| image:: /_static/images/en-us_image_0000001072358973.png
