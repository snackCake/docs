# Oracle DB

1. Download and install Oracle (http://www.oracle.com/technetwork/database/enterprise-edition/overview/index.html)

2. Create a new database and a user capable of accessing this database with privileges for creating tables included (see Oracle documentation if you have questions about how to administrate databases and users).

3. Download the Oracle JDBC driver (http://www.oracle.com/technetwork/database/features/jdbc/index-091264.html)

4. Follow the instructions for your application server for creating a JNDI resource(s). Note that the driver does not go inside the war file(s). Rather it must go on the class path of the server.

5. Update the runtime properties to use the correct dialect for the Oracle Server. (see [[Database Configuration]]).

    ```ini
    blPU.hibernate.dialect="org.hibernate.dialect.Oracle10gDialect
    blSecurePU.hibernate.dialect="org.hibernate.dialect.Oracle10gDialect
    blCMSStorage.hibernate.dialect="org.hibernate.dialect.Oracle10gDialect
    ```

6. If you are only using Oracle in all environments and you wish to utilize the heat clinic demo import scripts, edit core/src/main/resources/runtime-properties/common-shared.properties. Add the following property to cause the system to modify the import scripts at runtime for Oracle compatibility:

    ```ini
    blPU.hibernate.hbm2ddl.import_files_sql_extractor=org.broadleafcommerce.common.util.sql.importsql.DemoOracleSingleLineSqlCommandExtractor
    ```
    
7. If the Jobs and Events module (commercial module) is in play as well, you would define this property as well:

    ```ini
    blEventPU.hibernate.hbm2ddl.import_files_sql_extractor=org.broadleafcommerce.common.util.sql.importsql.DemoOracleSingleLineSqlCommandExtractor
    ```

8. If you are using different database platforms in different environments, then you can move the config from #6 to an environment specific property file (e.g. development-shared.properties).
