:original_name: opengauss_06_0004.html

.. _opengauss_06_0004:

Key Operations Supported by CTS
===============================

Cloud Trace Service (CTS) records operations associated with GaussDB for later query, audit, and backtrack.

.. table:: **Table 1** Operations supported by CTS

   +---------------------------------------------------------------+---------------+----------------------------+
   | Operation                                                     | Resource Type | Trace Name                 |
   +===============================================================+===============+============================+
   | Creating a DB instance or restoring data to a new DB instance | instance      | createInstance             |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Deleting a DB instance                                        | instance      | deleteInstance             |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Changing instance specifications                              | instance      | resizeFlavor               |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Upgrading the instance version                                | instance      | upgradeVersion             |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Resetting a password                                          | instance      | resetPassword              |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Rebooting a DB instance                                       | instance      | instanceRestart            |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Modifying resource tags                                       | instance      | modifyTag                  |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Deleting resource tags                                        | instance      | deleteTag                  |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Adding resource tags                                          | instance      | createTag                  |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Renaming a DB instance                                        | instance      | instanceRename             |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Adding nodes                                                  | instance      | instanceAction             |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Deleting task records                                         | instance      | deleteTaskRecord4OpenGauss |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Reducing the number of replicas                               | instance      | reduceReplica              |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Deleting coordinator nodes                                    | instance      | reduceCoordinatorNode      |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Modifying the recycling policy                                | backup        | setRecyclePolicy           |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Creating a manual backup                                      | backup        | createManualSnapshot       |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Deleting a manual backup                                      | backup        | deleteManualSnapshot       |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Modifying the backup policy                                   | backup        | setBackupPolicy            |
   +---------------------------------------------------------------+---------------+----------------------------+
   | Restoring a DB instance                                       | backup        | restoreInstance            |
   +---------------------------------------------------------------+---------------+----------------------------+
