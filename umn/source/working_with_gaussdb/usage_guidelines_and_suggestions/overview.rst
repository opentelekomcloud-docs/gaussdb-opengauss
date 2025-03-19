:original_name: opengauss_tips_0001.html

.. _opengauss_tips_0001:

Overview
========

Introduction
------------

This section describes the database-related design and development specifications in the product design and development process based on the product lifecycle.

It aims to improve the readability and code quality, and emphasizes the practicability and operability. It also specifies the issues that may occur during GaussDB development. The following contents are included:

Design specifications in terms of database and performance

Programming specifications, including formatting, naming, comments, syntax, scripts, and database programming

Detailed rules and specific examples for some specifications if necessary

Terms
-----

This section uses the following terms for description:

-  **Specification**: database specification, which must be complied with during programming and design. Otherwise, the database reports an error.
-  **Rule**: a convention that must be complied with during programming and design.
-  **Recommendation**: a convention that is recommended to be considered during programming and design.
-  **Description**: explanation of the rule or recommendation in question.
-  **Example**: a positive or negative example of the rule or recommendation.
-  **Sharding**: Data in a table is split based on a specified policy and stored in tables with the same name on multiple DN shards. The data stored in these tables does not overlap with each other, which can be considered as horizontal sharding of tables, for example, the **t_user** table is split into four DN shards of GaussDB by hashing the primary key **userId**. Each shard has a **t_user** table with the same name.

Application Scope
-----------------

The usage guidelines and suggestions are suitable for GaussDB 1.x and later.

They are intended for designers, developers, development database administrators (DBAs), operation and maintenance (O&M) DBAs, and O&M personnel.
