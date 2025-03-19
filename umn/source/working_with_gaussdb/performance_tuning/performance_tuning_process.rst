:original_name: opengauss_opti_0020.html

.. _opengauss_opti_0020:

Performance Tuning Process
==========================

To fine-tune GaussDB performance, you need to identify performance bottlenecks, adjust key parameters, and optimize SQL statements. During the tuning, factors such as system resources, throughput, and loads can be used to locate and analyze performance problems to ensure that the system performance is acceptable.

Various factors must be considered during GaussDB performance tuning. Therefore, relevant personnel must know well about knowledge, such as system software architecture, hardware and software configuration, database parameter configuration, concurrency control, query processing, and database applications.

.. important::

   Performance tuning may need to reboot the instance and interrupt services. Therefore, after the service goes live and when the instance needs to be rebooted, you must send the request to related management department about the operation window time for approval.

Tuning Process
--------------

:ref:`Figure 1 <en-us_topic_0000002124277117___d0e35017>` shows the tuning process.

.. _en-us_topic_0000002124277117___d0e35017:

.. figure:: /_static/images/en-us_image_0000002088677854.png
   :alt: **Figure 1** GaussDB performance tuning process

   **Figure 1** GaussDB performance tuning process
