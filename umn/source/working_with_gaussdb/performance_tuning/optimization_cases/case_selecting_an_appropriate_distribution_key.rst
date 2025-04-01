:original_name: opengauss_opti_0066.html

.. _opengauss_opti_0066:

Case: Selecting an Appropriate Distribution Key
===============================================

Symptom
-------

Tables are defined as follows:

::

   CREATE TABLE t1 (a int, b int);
   CREATE TABLE t2 (a int, b int);

The following query is executed:

::

   SELECT * FROM t1, t2 WHERE t1.a = t2.b;

Optimization Analysis
---------------------

If **a** is the distribution key of **t1** and **t2**:

::

   CREATE TABLE t1 (a int, b int) DISTRIBUTE BY HASH (a);
   CREATE TABLE t2 (a int, b int) DISTRIBUTE BY HASH (a);

Then **Streaming** exists in the execution plan and the data volume is heavy among DNs, as shown in :ref:`Figure 1 <en-us_topic_0000002088677706__en-us_topic_0073253824_en-us_topic_0040046521_fig21969731>`.

.. _en-us_topic_0000002088677706__en-us_topic_0073253824_en-us_topic_0040046521_fig21969731:

.. figure:: /_static/images/en-us_image_0000002124277861.png
   :alt: **Figure 1** Selecting an appropriate distribution key (1)

   **Figure 1** Selecting an appropriate distribution key (1)

If **a** is the distribution key of **t1** and **b** is the distribution key of **t2**:

::

   CREATE TABLE t1 (a int, b int) DISTRIBUTE BY HASH (a);
   CREATE TABLE t2 (a int, b int) DISTRIBUTE BY HASH (b);

Then **Streaming** does not exist in the execution plan, and the data volume among DNs is decreasing and the query performance is increasing, as shown in :ref:`Figure 2 <en-us_topic_0000002088677706__en-us_topic_0073253824_en-us_topic_0040046521_fig63509856>`.

.. _en-us_topic_0000002088677706__en-us_topic_0073253824_en-us_topic_0040046521_fig63509856:

.. figure:: /_static/images/en-us_image_0000002088518278.png
   :alt: **Figure 2** Selecting an appropriate distribution key (2)

   **Figure 2** Selecting an appropriate distribution key (2)
