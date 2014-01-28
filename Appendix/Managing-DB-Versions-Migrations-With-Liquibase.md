# Managing Database Versions and Migrations with Liquibase

Liquibase (http://www.liquibase.org/index.html) is a very powerful Database versioniong and management tool that we highly recommend you consider using to help manage your Database upgrades and migrations.

Depending on your development and release processes, you may not want Liquibase to directly update your database. Reasons for this may include a desire to tweak the resulting SQL, have the SQL approved by a DBA, or for SOX compliance. For this reason, all update and rollback Liquibase commands have a “sql output” mode which do not execute anything against the database but instead save the resulting SQL to standard out or a specified file which you can also then review with your DBA before applying the final script.

Please read the Liquibase documentation for more details on best practices.

## Upgrading the Heat Clinic from one version to another using Liquibase

If you are already using Liquibase in your current setup, we'll provide you with a sample database changelog file that
you can use as a reference when upgrading versions of Broadleaf. You can reference this file in the migration section of the documentation.

If you aren't using Liquibase, here is an example of incorporating the Liquibase Maven plugin into the DemoSite. This will will show you how to create a Liquibase database changelog file that will upgrade your Broadleaf 3.0.x schema to the new schema required for Broadleaf 3.1.x. Of course, this can apply to upgrading from any version to another, for the purposes of this example, we will show you how to upgrade from 3.0 to 3.1

>> Note: This example implementation requires that you create a reference Database of the newest version (e.g. 3.1 schema) so that Liquibase can connect to it in order to generate the database diff. It, is also recommended that the two databases that you compare are of the same type. Even though the Liquibase changelog file is database agnostic, when you are comparing two databases of different types (e.g. HSQL and MySQL) you may get un-intended diff comparisons if they aren't of the same type.

1) If you don't have a reference DB set up of the new schema, you can clone the repo of the new version of the DemoSite and set the Hibernate properties file to `create` instead of `create-drop`, start-up the application, and create the reference Database. 

Change your development-shared.properties and development.properties of the upgraded application to create a new reference schema: (3.1 for this example)

```properties
blPU.hibernate.hbm2ddl.auto=create
```

2) Once you have a reference DB of the new schema created, switch back to the old schema and application and add the Liquibase Maven Plugin so you can do a Database Diff

3) Add the Liquibase Maven Plugin to your root `pom.xml`

```xml
  <dependency>
      <groupId>org.liquibase</groupId>
      <artifactId>liquibase-maven-plugin</artifactId>
      <version>3.0.6</version>
  </dependency>
```

4) Add the dependency to your `core` pom as well

```xml
  <dependency>
      <groupId>org.liquibase</groupId>
      <artifactId>liquibase-maven-plugin</artifactId>
  </dependency>
```

5) Add the Liquibase Maven Plugin configuration in your `core` pom.xml. You can find more details about the plugin configuration here (http://www.liquibase.org/documentation/maven/)

In this example, you must configure the following properties:

- changeLogFile: the path to the changelog file that Liquibase will use to generate any migration SQL scripts
- diffChangeLogFile: the path to the file that Liquibase will generate the database diff changelog file
- driver: the database driver of the source DB (3.0 schema for this example)
- username: the user name for the source DB (3.0 schema for this example)
- password: the password for the source DB (3.0 schema for this example)
- url: the JDBC url for the source DB (3.0 schema for this example)
- referenceDriver: the database driver for the reference DB which to compare to (3.1 schema for this example)
- referenceUsername: the user name for the reference DB (3.1 schema for this example)
- referencePassword: the password for the reference DB (3.1 schema for this example)
- referenceUrl: the JDBC url for the reference DB (3.1 schema for this example)

```xml
  <plugin>
      <groupId>org.liquibase</groupId>
      <artifactId>liquibase-maven-plugin</artifactId>
      <version>3.0.6</version>
      <executions>
          <execution>
              <goals><goal>status</goal></goals>
          </execution>
      </executions>
      <configuration>
          <changeLogFile>src/main/resources/db.changelog-NEW.xml</changeLogFile>
          <diffChangeLogFile>src/main/resources/db.changelog-NEW.xml</diffChangeLogFile>
          <driver>com.mysql.jdbc.Driver</driver>
          <username>mysqlUserName</username>
          <password>mysqlPassword</password>
          <url>jdbc:mysql://localhost:3306/broadleaf_3_0_x</url>
          <referenceDriver>com.mysql.jdbc.Driver</referenceDriver>
          <referenceUsername>mysqlUserName</referenceUsername>
          <referencePassword>mysqlPassword</referencePassword>
          <referenceUrl>jdbc:mysql://127.0.0.1:3306/broadleaf_3_1_x</referenceUrl>
          <promptOnNonLocalDatabase>false</promptOnNonLocalDatabase>
      </configuration>
      <dependencies>
          <dependency>
              <groupId>mysql</groupId>
              <artifactId>mysql-connector-java</artifactId>
              <version>5.1.6</version>
          </dependency>
      </dependencies>
  </plugin>
```

6) Do a `mvn clean install` on your DemoSite project to pull in all the dependencies, and cd into your `core` directory and type in:

```text
mvn liquibase:diff
```

7) After executing the above command, it should generate a changelog file in the directory specified above. In this example: `src/main/resources/db.changelog-NEW.xml`

8) Once you have reviewd the diff of the new schema changes, make any adjustments, and then you can get liquibase to generate the migration SQL script for you. Run the following command in your `core` directory"

```text
mvn liquibase:updateSQL
```

This should generate a migrate.sql script in your `core/target` directory by default. You can use this as a reference of the DB changes that are required to upgrade to the newest version.


