:original_name: opengauss_tips_0014.html

.. _opengauss_tips_0014:

Function/Stored Procedure Design Guidelines
===========================================

-  Do not use stored procedures or triggers to implement service logic. Instead, process the logic on the service server to prevent logical dependency on the database.

-  Do not use stored procedures to implement the upgrade logic in the database upgrade scripts of a service.

-  Create functions that have fixed return values for fixed input parameters and set the function type to IMMUTABLE or SHIPPABLE.

   Currently, the database supports three types of functions: IMMUTABLE, STABLE, and VOLATILE.

   If the attribute of an IMMUTABLE function is set to **SHIPPABLE**, the function can be executed on DNs. In most cases, such functions perform efficiently.

   However, such functions require fixed return values for fixed input parameters to ensure that the functions are correctly executed on DNs. If the function execution result depends on the table scan result (for example, maximum value of a column in the table) or scan time (for example, current time), set the function type to STABLE or VOLATILE, and set the function attribute to NOT SHIPPABLE to ensure that the function is correctly executed. In this scenario, data on all DNs is sent to one CN for computing, resulting in low query efficiency.
