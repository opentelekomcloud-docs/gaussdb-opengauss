:original_name: opengauss_07_0003.html

.. _opengauss_07_0003:

Creating a Custom Policy
========================

Custom policies can be created to supplement the system-defined policies of GaussDB. For the actions supported for custom policies, see the section "Permissions Policies and Supported Actions" in *API Reference*.

You can create custom policies in either of the following two ways:

-  Visual editor: Select cloud services, actions, resources, and request conditions. This does not require knowledge of policy syntax.
-  JSON: Create a policy in JSON format or edit the JSON strings of an existing policy.

For details about how to create a custom policy, see the section "Identity and Access Management" > "Permissions"> "Custom Policies". The following contains examples of common GaussDB custom policies.

Example Custom Policy
---------------------

-  Example 1: Allowing users to create GaussDB instances

   .. code-block:: text

      {
          "Version": "1.1",
          "Statement": [{
              "Effect": "Allow",
              "Action": ["gaussdb:instance:create"]
              }]
      }

-  Example 2: Denying GaussDB instance deletion

   A policy with only "Deny" permissions must be used in conjunction with other policies. If the permissions assigned to a user include both "Allow" and "Deny", the "Deny" permissions take precedence over the "Allow" permissions.

   The following method can be used if you need to assign permissions of the **GaussDB FullAccess** policy to a user but you want to prevent the user from deleting GaussDB instances. Create a custom policy for denying GaussDB instance deletion, and attach both policies to the group to which the user belongs. Then, the user can perform all operations on GaussDB instances except deleting GaussDB instances. The following is an example of a deny policy:

   .. code-block:: text

      {
          "Version": "1.1",
          "Statement": [{
              "Action": ["gaussdb:instance:delete"],
              "Effect": "Deny"
          }]
      }
