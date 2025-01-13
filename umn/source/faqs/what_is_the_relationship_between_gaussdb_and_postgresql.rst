:original_name: opengauss_faq_0012.html

.. _opengauss_faq_0012:

What is the Relationship Between GaussDB and PostgreSQL?
========================================================

Based on the Postgres-XC architecture of open-source PostgreSQL 9.2, the earliest GaussDB kernel developed a multi-CN architecture, on which some major features, such as distributed execution framework (stream operators) and vectorized engine, are provided. Currently, GaussDB only uses standard APIs and common functions of PostgreSQL. It has developed its own ecosystem openGauss, architecture, and key technologies. Its centralized instances are open-source, and storage engine and optimizer have been rearchitected. The differences between GaussDB and PostgreSQL are as follows:

-  PostgreSQL uses a process model, and GaussDB uses a thread pool model.
-  PostgreSQL supports only row-store. GaussDB supports row-store, column-store, and Ustore.
-  PostgreSQL supports only centralized deployment. GaussDB supports both centralized and distributed deployment.
-  Compared with PostgreSQL, GaussDB has many unique features, such as dynamic masking, end-to-end encryption, anti-tampering, GTM-Lite mode, NUMA-aware architecture, and geo-redundant and intra-city dual-cluster solutions.
