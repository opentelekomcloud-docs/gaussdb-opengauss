:original_name: opengauss_opti_0067.html

.. _opengauss_opti_0067:

Case: Creating an Appropriate Index
===================================

Symptom
-------

To query information about all employees, run the following statements:

::

   SELECT staff_id,first_name,last_name,employment_id,state_name,city
   FROM staffs,sections,states,places
   WHERE sections.section_name='Sales'
   AND staffs.section_id = sections.section_id
   AND sections.place_id = places.place_id
   AND places.state_id = states.state_id
   ORDER BY staff_id;

Optimization Analysis
---------------------

The original execution plan is as follows before creating the **places.place_id** and **states.state_id** indexes:

|image1|

The optimized execution plan is as follows (two indexes have been created on the **places.place_id** and **states.state_id** columns):

|image2|

.. |image1| image:: /_static/images/en-us_image_0000002124197501.jpg
.. |image2| image:: /_static/images/en-us_image_0000002088678058.jpg
