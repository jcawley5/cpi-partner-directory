# cpi-partner-directory

## Create Service Credential

- In the BTP cockpit choose **Services -> Service Marketplace**
- Choose the service **Process Integration Runtime**
- Choose **Create** and create a service with the following details
  - Plan: **api**
  - Roles: **AuthGroup_TenantPartnerDirectoryConfigurator**
  - Grant-types: **Client Credentials**

## Setup Postman Environment

- Download the [CPI Tenant Template.postman_environment](./CPI Tenant Template.postman_environment.json)
