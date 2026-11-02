:original_name: opengauss_faq_0010.html

.. _opengauss_faq_0010:

What Do I Do If Replay Speed of Standby Nodes Cannot Catch Up with Write Speed of Primary Nodes?
================================================================================================

Symptom
-------

When workloads on a DB instance are heavy, the replay speed of standby nodes cannot catch up with write speed of primary nodes. After the system runs for a long time, logs are accumulated on the standby nodes. If a primary node is faulty, data restoration takes a long time and the database is unavailable, severely affecting system availability.

Solution
--------

GaussDB provides ultimate RTO to minimize the data recovery time after a primary node is faulty and improve availability.

To use ultimate RTO, submit an application by choosing in the upper right corner of the console.

Precautions
-----------

-  Ultimate RTO focuses only on whether the RTO of standby nodes meets requirements.
-  Enabling ultimate RTO consumes CPU and memory resources of standby nodes.
-  In 1.4 and earlier versions, flow control takes effect when ultimate RTO is enabled.
-  Standby nodes do not support read operations after ultimate RTO is enabled. If you send read operations to a standby node, the standby node may fail to return the results.
