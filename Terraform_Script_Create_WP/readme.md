# WordPress on Linux App Service Terraform Deployment

* Time to deploy - Approx. 8 minutes

This is the Terraform template that deploys the resources for running a WordPress site on a Linux App service.
It deploys the following resources:
* App Service Hosting Plan - Provisoned to use a WordPress container image based on the alpine latest build.
* App Service - Configured to use a WordPress container image based on the alpine latest build.
* MySQL Server running on the Flexible server SKU with a database deployed and configured for use with WordPress.
* Virtual Network - with a 10.0.0.0/16 CIDR block.
* Private DNS Zone for the MySQL database.
* CDN Profile (Azure Frontdoor) with a CDN profile endpoint with compression enabled.


## Overview
The main Terraform template (main.tf) is broken up into easy to read and modify sections and is very configurable via custom parameters that are well documented. You can deploy this template without changing any parameters but will be promped for database server password and wordpress password.
There is also a terraform variables file (dev.tfvars) included in the repository which you can change the values to suit your deployment. Please remember to **NOT** store usernames or passwords in this file. The variables file should be self expanatory as the parameters are names in accordance with their purpose.

## How to deploy
Firstly, modify the variable file (dev.tfvars) with the parameter input values for your environment, its accepted practice to have a variables file per environment/deployment as needed.

From there run the following commands:

* **tf init** - To download and initialize the Terraform providers locally.
* **tf plan -var-file='dev.tfvars'** - To show changes required by the current configuration, passing in the file 'dev.tfvars' as input. You will be prompted for passwords for the MySQL and Wordpress installations.
* **tf apply -var-file='dev.tfvars'** - To deploy the infrastructure, passing in the file 'dev.tfvars' as input. You will be prompted for passwords for the MySQL and Wordpress installations. Once you have checked the deployment - Confirm creation by entering 'Yes'.
* **tf destroy -var-file='dev.tfvars'** - To destroy previously-created infrastructure. You will be prompted for passwords for the MySQL and Wordpress installations. Once you have checked the deployment - Confirm destruction by entering 'Yes'.

**KNOWN ISSUES**
* There is an issue with the CDN Endpoint not setting the Origin hostname to an App service properly. As a Temporary fix you will need to go into the CDN Endpoint settings and change the Origin Type to Web App and change the Origin hostname and host header to the Web app url that was deployed earlier (*.azurewebsites.net). You will also need to purge all content from the endpoint to effect the change. This is being investigated and a bugfix is being worked on.