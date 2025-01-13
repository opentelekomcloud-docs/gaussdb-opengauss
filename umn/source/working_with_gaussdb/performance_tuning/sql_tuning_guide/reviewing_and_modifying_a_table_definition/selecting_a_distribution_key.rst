:original_name: opengauss_opti_0040.html

.. _opengauss_opti_0040:

Selecting a Distribution Key
============================

Using the following principles to select a distribution key for a hash table:

#. **The values of the distribution column should be discrete so that data can be evenly distributed on each DN.** You can select the primary key of the table as the distribution key. For example, for a person information table, choose the ID number column as the distribution key.
#. **Do not select the column that has a constant filter.** If a constant constraint exists on the **zqdh** column (for example, zqdh= '000001') in some queries on the **dwcjk** table, you are not advised to use **zqdh** as the distribution column.
#. **With the above principles met, you can select join conditions as distribution keys**, so that join tasks can be pushed down to DNs for execution, reducing the amount of data transferred between the DNs.

For a hash table, an inappropriate distribution key may cause data skew or poor I/O performance on certain DNs. You need to check the table to ensure that data is evenly distributed on each DN. You can run the following SQL statements to check data skew:

.. code-block::

   select
   xc_node_id, count(1)
   from tablename
   group by xc_node_id
   order by xc_node_id desc;

**xc_node_id** corresponds to a DN. Generally, **over 5% difference between the amount of data on different DNs is regarded as data skew. If the difference is over 10%, choose another distribution key.**

Multiple distribution keys can be selected in GaussDB to evenly distribute data.
