# Managing Database Versions and Migrations with Liquibase

Liquibase (http://www.liquibase.org/index.html) is a very powerful Database versioniong and management tool that we highly recommend you consider using to help manage your Database upgrades and migrations.

Depending on your development and release processes, you may not want Liquibase to directly update your database. Reasons for this may include a desire to tweak the resulting SQL, have the SQL approved by a DBA, or for SOX compliance. For this reason, all update and rollback Liquibase commands have a “sql output” mode which do not execute anything against the database but instead save the resulting SQL to standard out or a specified file which you can also then review with your DBA before applying the final script.

Please read the Liquibase documentation for more details on best practices.

## Upgrading the Heat Clinic from one version to another

If you are already using Liquibase in your current setup, we'll provide you with a sample database changelog file that
you can use as a reference when upgrading versions of Broadleaf. You can reference this file in the migration section of the documentation.

If you would like to generate the Database changelog yourself, you can include the Liquibase maven dependencies in your Demo Site and perform the following:

