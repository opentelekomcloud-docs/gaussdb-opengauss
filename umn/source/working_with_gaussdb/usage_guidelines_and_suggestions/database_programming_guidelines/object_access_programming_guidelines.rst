:original_name: opengauss_tips_0016.html

.. _opengauss_tips_0016:

Object Access Programming Guidelines
====================================

You need to use *schemaname.tablename* to access objects (tables and functions).

If the schema name prefix is not added, the system searches all tablespaces in the current search path until a matched table is found as the target table, which causes unnecessary performance overhead.
