:original_name: opengauss_api_0031.html

.. _opengauss_api_0031:

Abnormal Request Results
========================

**Abnormal response description**

.. table:: **Table 1** Abnormal response description

   +------------+--------+------------------------------------------------------------------------------------------+
   | Name       | Type   | Description                                                                              |
   +============+========+==========================================================================================+
   | error_code | String | Specifies the error returned when a task submission exception occurs.                    |
   +------------+--------+------------------------------------------------------------------------------------------+
   | error_msg  | String | Specifies the description of the error returned when a task submission exception occurs. |
   +------------+--------+------------------------------------------------------------------------------------------+

**Response example**

.. code-block:: text

   {
       "error_code": "DBS.200022",
       "error_msg": "The DB instance name already exists."
   }
