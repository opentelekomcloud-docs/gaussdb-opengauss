:original_name: opengauss_tips_0010.html

.. _opengauss_tips_0010:

Permission Design Guidelines
============================

-  Before using a service, the **root** user must create DATABASE, SCHEMA, and USER for the service, and then grant object permissions to related users.

   .. note::

      If a user is not the schema owner and wants to access objects in the schema, the user must be granted the schema usage permission and object permissions.

-  Use lowercase letters for the names of DATABASE, SCHEMA, and USER.

   .. note::

      By default, the database converts the object name to lowercase letters. If the object name in the connection string contains uppercase letters, the database cannot be connected. For example, you cannot connect to the database using the user **MyUser**, instead of using **myuser**. To avoid confusion, use lowercase letters.

-  Assign permissions to roles and users based on the least privilege principle.

   .. table:: **Table 1** Permissions on database objects

      +------------+-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Object     | Permission                        | Description                                                                                                                                             |
      +============+===================================+=========================================================================================================================================================+
      | Database   | CONNECT                           | Allows users to connect to the database.                                                                                                                |
      +------------+-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      |            | TEMP                              | ``-``                                                                                                                                                   |
      +------------+-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      |            | CREATE                            | Allows schemas to be created within the database.                                                                                                       |
      +------------+-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Schema     | CREATE                            | Allows new objects to be created within the schema.                                                                                                     |
      +------------+-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      |            | USAGE                             | Allows access to objects contained in the specified schema. Without this permission, it is still possible to see the object names.                      |
      +------------+-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Function   | EXECUTE                           | Allows calling a function, including use of any operators that are implemented on top of the function.                                                  |
      +------------+-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Tablespace | CREATE                            | Allows tables to be created within the tablespace, and allows databases and schemas to be created that have the tablespace as their default tablespace. |
      +------------+-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Table      | INSERT, DELETE UPDATE, and SELECT | Allows users to add, delete, modify, and query specified tables.                                                                                        |
      +------------+-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      |            | TRUNCATE                          | Allows truncating all records in specified tables.                                                                                                      |
      +------------+-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
      |            | REFERENCES                        | Allows creation of a foreign key constraint referencing a table. This permission is required on both referencing and referenced tables.                 |
      +------------+-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+

-  Manage permissions based on roles instead of users.

   Configure permissions in roles and grant roles to a user.

   Use roles to manage permissions if multiple users and user changes are involved. Example:

   -  Roles and users are in many-to-many relationship. A role can be granted to multiple users. If permissions of a role are modified, the permissions of the granted users can be updated at the same time.
   -  Deleting a user does not affect the role.
   -  A new user can quickly obtain required permissions by granting a role to the user.

-  When deleting a specified database, revoke the CONNECT permission of users on the database to prevent deletion failures caused by active database connections.
