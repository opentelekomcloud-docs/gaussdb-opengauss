:original_name: opengauss_opti_0041.html

.. _opengauss_opti_0041:

Using Partial Cluster Key
=========================

Partial cluster key is the column-store-based technology. It can minimize or maximize sparse indexes to quickly filter base tables. Partial cluster key can specify multiple columns, but you are advised to specify no more than two columns. Use the following principles to specify columns:

#. The selected columns must be restricted by simple expressions in base tables. Such constraints are usually represented by Col, Op, and Const. Col specifies the column name, Op specifies operators, (including =, >, >=, <=, and <), and Const specifies constants.
#. Select columns that are frequently selected (to filter much more undesired data) in simple expressions.
#. List the less frequently selected columns on the top.
#. List the columns of the enumerated type at the top.
