# MS SQL Server

1. Download and install SQL Server (http://www.microsoft.com/sqlserver/en/us/editions/express.aspx)

2. Create a new database and a user capable of accessing this database with privileges for creating tables included (see SQL Server documentation if you have questions about how to administrate databases and users).

3. Download the SQL Server JDBC driver (http://msdn.microsoft.com/en-us/sqlserver/aa937724)

4. Follow the instructions for your application server for creating a JNDI resource(s). Note that the driver does not go inside the war file(s). Rather it must go on the class path of the server.

5. Update the runtime properties to use the correct dialect for the MS SQL Server. (see [[Database Configuration]]).

    ```ini
    blPU.hibernate.dialect="org.hibernate.dialect.SQLServer2008Dialect
    blSecurePU.hibernate.dialect="org.hibernate.dialect.SQLServer2008Dialect
    blCMSStorage.hibernate.dialect="org.hibernate.dialect.SQLServer2008Dialect
    ```

6. If you are only using MS SQL Server in all environments and you wish to utilize the heat clinic demo import scripts, edit core/src/main/resources/runtime-properties/common-shared.properties. Add the following property to cause the system to modify the import scripts at runtime for SQL Server compatibility:

    ```ini
    blPU.hibernate.hbm2ddl.import_files_sql_extractor=org.broadleafcommerce.common.util.sql.importsql.DemoSqlServerSingleLineSqlCommandExtractor
    blEventPU.hibernate.hbm2ddl.import_files_sql_extractor=org.broadleafcommerce.common.util.sql.importsql.DemoSqlServerSingleLineSqlCommandExtractor
    ```
    
7. If you are using different database platforms in different environments, then you can move the config from #6 to an environment specific property file (e.g. development-shared.properties).

8. Check the JDK version you are using.

    > **JDK 1.6.0_29 is not compatible with the SQL Server JDBC driver**

    > **Startup will appear to hang on database connect when using 1.6.0_29**

    > **1.6.0_30 and above fixes the issue**
