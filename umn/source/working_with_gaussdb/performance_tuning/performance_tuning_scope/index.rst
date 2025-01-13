:original_name: opengauss_opti_0021.html

.. _opengauss_opti_0021:

Performance Tuning Scope
========================

If you are not satisfied with the service execution efficiency and want to improve the efficiency, you can tune database performance. The database performance is affected by many factors as described in section :ref:`Performance Tuning Scope <opengauss_opti_0021>`. Performance tuning is a complex process and sometimes cannot be systematically described or explained. It depends more on the database administrator's experience. However, this section still attempts to illustrate the performance tuning methods for application development personnel and new GaussDB database administrators.

Performance Factors
-------------------

Factors that affect database performance are as follows:

-  System resources

   Database performance greatly depends on the I/O and memory usage of disks. To accurately configure performance metrics, you need to have a knowledge of the basic performance of the hardware deployed in the instance. Performance of hardware, such as vCPUs, hard disks, disk controllers, memory, and network interfaces will significantly affect the database execution speed.

-  Load

   The load indicates the total database system demands and it changes over time. The overall load contains user queries, applications, concurrent jobs, transactions, and system commands transferred at any time. For example, the system load increases if multiple users are executing multiple queries. The load will significantly affect the database performance. Identifying load peak hours helps improve resource utilization so that tasks are executed effectively.

-  throughput

   The data processing capability of a database is defined by its throughput. Database throughput is measured in queries per second, transactions per second, or average response time. The database processing capacity is closely related to the underlying system performance (disk I/O, CPU speed, and storage bandwidth). You need to know about the hardware performance before setting a target throughput.

-  Competition

   Competition indicates that two or more load components try to use system resources in a conflicting way. For example, competition occurs when multiple queries attempt to update the same data at the same time, or when a large number of loads compete for system resources. When competition increases, the throughput decreases.

-  Tuning

   The database tuning can affect the performance of the whole system. To obtain an optimal execution plan, the query optimizer is used for SQL customization, database parameter settings, table design, and data allocation.

Determining the Tuning Scope
----------------------------

Performance tuning depends on the usage of hardware resources, such as the CPU, memory, I/O, and network of each node in an instance. Check whether these resources are fully utilized, and whether any bottlenecks exist, and then perform performance tuning as required.

-  If a resource reaches the bottleneck:

   #. Find the resource consuming SQL statements by querying the most time-consuming SQL statements and unresponsive SQL statements
   #. Optimize these SQL statements by referring to :ref:`SQL Tuning Guide <opengauss_opti_0029>`.

-  If no resource reaches the bottleneck, the system performance can be improved. In this case, query the most time-consuming SQL statements and the unresponsive SQL statements, and then perform :ref:`SQL Tuning Guide <opengauss_opti_0029>` as required.

-  :ref:`Querying the Most Time-Consuming SQL Statements <opengauss_opti_0027>`
-  :ref:`Checking Blocked Statements <opengauss_opti_0028>`
-  :ref:`Tuning Parameters <opengauss_parameter_optimization>`

.. toctree::
   :maxdepth: 1
   :hidden: 

   querying_the_most_time-consuming_sql_statements
   checking_blocked_statements
   tuning_parameters
