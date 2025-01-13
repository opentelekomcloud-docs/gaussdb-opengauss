:original_name: opengauss_01_0064.html

.. _opengauss_01_0064:

Configuring an Automated Backup Policy for Instances
====================================================

Scenarios
---------

When you create a GaussDB instance, an instance-level automated backup policy is enabled by default. After your instance is created, you can modify the automated backup policy as needed. GaussDB backs up data based on the automated backup policy you specified.

The automated backup policy is enabled by default as follows:

-  Retention period: 7 days.
-  Time window: A one-hour period the backup will be scheduled for each day, such as **01:00-02:00** or **12:00-13:00**. The backup time is in UTC format. If the DST or standard time is switched, the backup time window changes with the time zone.
-  Backup cycle: every day by default.
-  Differential backup policy: Backup files are saved every 30 minutes by default.
-  Backup flow control: The default value is **75 MB/s**.
-  Prefetch pages: The default value is **64**.

.. note::

   To ensure that data can be restored to a point in time, the latest full backup that exceeds the backup retention period will not be deleted immediately. For example, if **Backup Cycle** is set to **All** and **Retention Period** to one day and backup 1 is generated on November 1, this backup will not be deleted on November 2 when backup 2 is generated, but will be deleted on November 3 when backup 3 is generated.

Constraints
-----------

Rebooting the instance is not allowed during full backup. Exercise caution when selecting a backup time window.

Modifying an Automated Backup Policy
------------------------------------

#. Log in to the management console.
#. Click |image1| in the upper left corner and select a region and project.
#. Click |image2| in the upper left corner of the page and choose **Databases** > **GaussDB**. The GaussDB console is displayed.
#. On the **Instances** page, click the name of the target instance to go to the **Basic Information** page.
#. In the navigation pane on the left, choose **Backups**. On the displayed page, click **Modify Backup Policy**. You can view the configured backup policy. To modify the backup policy, adjust the parameter values as needed.
#. Configure parameters.

   -  Full backup policy:

      -  **Retention Period**: Specify **Retention Period**, which indicates the number of days that your automated backups can be retained. Increasing the retention period will improve data reliability. However, even if the retention period has expired, the most recent backup will be retained.

         -  Extending the retention period improves data reliability. You can extend the retention period as needed.
         -  If you shorten the retention period, the new backup policy takes effect for existing backups. Any automated backups (including full and incremental backups) that have expired will be automatically deleted. Manual backups will not be automatically deleted but you can delete them manually.

         **Policy for automatically deleting automated full backups:**

         To ensure data integrity, even after the retention period expires, the most recent backup will be retained.

         If **Backup Cycle** was set to **Monday** and **Tuesday** and the **Retention Period** was set to **2**:

         -  The full backup generated on Monday will be automatically deleted on Thursday. The reasons are as follows:

            The backup generated on Monday expires on Wednesday, but it was the last backup, so it will be retained until a new backup expires. The next backup will be generated on Tuesday and will expire on Thursday. So the full backup generated on Monday will not be automatically deleted until Thursday.

         -  The full backup generated on Tuesday will be automatically deleted on the following Wednesday. The reasons are as follows:

            The backup generated on Tuesday will expire on Thursday, but as it is the last backup, it will be retained until a new backup expires. The next backup will be generated on the following Monday and will expire on the following Wednesday, so the full backup generated on Tuesday will not be automatically deleted until the following Wednesday.

      -  **Backup Flow Control**: Set the upload speed when backing up data to OBS, in MB/s. The default value is **75**. The value **0** indicates that the speed is not limited.

      -  **Time Window**: Select a one-hour period the backup will be scheduled for each day, such as **01:00-02:00** or **12:00-13:00**. The backup time is in UTC format. If the DST or standard time is switched, the backup time window changes with the time zone.

      -  **Backup Cycle**: Select at least one day as required.

         .. note::

            The backup retention period is from 1 to 732 days.

            The time window is one hour. You are advised to select an off-peak time window for full backups. By default, each day of the week is selected for **Backup Cycle**. You can change the backup cycle. At least one day must be selected.

            A full backup is immediately triggered after a DB instance is created. Then, a full backup or differential backup is performed based on the time window and backup cycle you specified. We recommend that you set the full backup time window to an off-peak hour.

   -  Differential backup policy:

      -  **Backup Cycle**: Select the backup cycle for performing a differential backup. The default value is **30 min**.

      -  **Prefetch Pages**: Set the number of prefetch pages from the modified pages in the disk table file during a differential backup. The default value is **64**. When modified pages are adjacent (for example, with a bulk data load), you can set this parameter to a large value. When modified pages are scattered (for example, random update), you can set this parameter to a small value. If this parameter is set to a large value, the occupied I/O increases. In this case, other services are affected and the database performance deteriorates.

      -  **Shard Size**: Backup files generated during full and differential backups are split based on the shard size. The value range is from 0 to 1024, but it must be a multiple of 4. The default value is **4**. **0** indicates that the size is not limited.

#. Click **OK**.

.. |image1| image:: /_static/images/en-us_image_0000002088517922.png
.. |image2| image:: /_static/images/en-us_image_0000002124197217.png
