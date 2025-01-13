:original_name: opengauss_opti_0037.html

.. _opengauss_opti_0037:

Reviewing and Modifying a Table Definition
==========================================

In a distributed framework, data is distributed on DNs. Data on one or more DNs is stored on a physical storage device. To properly define a table, you must:

#. **Evenly distribute data on each DN** to avoid the available capacity decrease of an instance caused by insufficient storage space of the storage device associated with a DN. Select a proper distribution column to avoid data skew.
#. **Evenly assign table scanning tasks on each DN** to avoid that a DN is overloaded by the table scanning tasks. Specifically, do not select columns in the equivalent filter of a base table as distribution keys.
#. **Reduce the data volume scanned** by using the partition pruning mechanism.
#. **Minimize random I/Os** by using clustering or partial clustering.
#. **Avoid data shuffle** to reduce the network pressure by selecting the **join-condition** or **group by** column as the distribution key.

Distributed keys are critical for defining a table. :ref:`Figure 1 <en-us_topic_0000002124277305___d0e37834>` shows the table definition process. The table definition is created during the database design and is reviewed and modified during the SQL statement optimization.

.. _en-us_topic_0000002124277305___d0e37834:

.. figure:: /_static/images/en-us_image_0000002124197345.png
   :alt: **Figure 1** Table definition process

   **Figure 1** Table definition process
