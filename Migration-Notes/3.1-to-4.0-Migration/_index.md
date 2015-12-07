#3.1 to 4.0.0-GA Migration

This document and related documents in the sidebar detail migration steps for getting your Broadleaf 3.1 site working with Broadleaf 4.0. Some disclaimers:

1. We assume that your project is currently on the most recent 3.1 version of Broadleaf
2. We assume that you have successsfully followed all migration and upgrade notes for the latest release version of Broadleaf 3.1
3. These migration notes are based off of the [DemoSite](https://github.com/BroadleafCommerce/DemoSite). To see all of the changes that were made to DemoSite in aggregate, check out the commmits made since 3.1.
4. You are running on a Postgres, MySQL, Oracle or SQL Server database

This migration document is divided into the following sections

- Changes to configuration in your site and admin projects
- REST API updates to remove Jersey in favor of Spring MVC
- Resource bundling updates
- Liquibase data migrations tested on our supported databases
    - Required column additions and changes
    - New table additions
    - Required data migration
    - Optional cleanup (e.g. deleting unused columns and tables)