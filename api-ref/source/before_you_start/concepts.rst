:original_name: opengauss_api_0006.html

.. _opengauss_api_0006:

Concepts
========

-  Domain

   A domain is created upon successful registration. The domain has full access permissions for all of its cloud services and resources. It can be used to reset user passwords and grant user permissions. The domain is a payment entity and should not be used directly to perform routine management. For security purposes, create IAM users and grant them permissions for routine management.

-  IAM User

   An IAM user is created using an account to use cloud services. Each IAM user has its own identity credentials (password and access keys).

   The domain name, username, and password will be required for API authentication.

-  Region

   A region is a geographic area in which cloud resources are deployed. Availability zones (AZs) in the same region can communicate with each other over an intranet, while AZs in different regions are isolated from each other. Deploying cloud resources in different regions can better suit certain user requirements or comply with local laws or regulations.

-  AZ

   An AZ contains one or more physical data centers. Each AZ has independent cooling, fire extinguishing, moisture-proof, and electricity facilities. Within an AZ, computing, network, storage, and other resources are logically divided into multiple clusters. AZs within a region are interconnected using high-speed optical fibers to support cross-AZ high-availability systems.

-  Project

   Projects group and isolate resources (including compute, storage, and network resources) across physical regions. A default project is provided for each region, and subprojects can be created under each default project. Users can be granted permissions to access all resources in a specific project. For more refined access control, create subprojects under a project and purchase resources in the subprojects. Users can then be assigned permissions to access only specific resources in the subprojects.


   .. figure:: /_static/images/en-us_image_0000001072791022.gif
      :alt: **Figure 1** Project isolating model

      **Figure 1** Project isolating model
