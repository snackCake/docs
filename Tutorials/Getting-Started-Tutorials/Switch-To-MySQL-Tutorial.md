# Switch To MySQL Tutorial

A very common step that users may want to do is switch away from the bundled HSQL database to a more mature database such as MySQL. We have a general guide on [[using MySQL | MySQL]], but this tutorial will be more explicit specifically regarding Heat Clinic.

1. Download and install MySQL Database (http://dev.mysql.com/downloads/mysql/)

1a. For proper UTF8 configuration and easy of use across multiple platforms (Linux, MySQL, Windows) we also recommend these minimum settings in `my.cnf`:

```ini
[mysqld]
lower_case_table_names=1
character-set-server=utf8
collation-server=utf8_general_ci
```


2. Create a new database and a user capable of accessing this database with privileges for creating tables included (see MySQL documentation if you have questions about how to administrate databases and users).

3. Update your pom.xml files to reference MySQL connectors. In the root `pom.xml`, you want to place the following in the `<dependencyManagement>` section:

    ```xml
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.26</version>
        <type>jar</type>
        <scope>compile</scope>
    </dependency> 
    ```

    and you'll want to place the following in the `<dependencies>` section of `admin/pom.xml` and `site/pom.xml`:

    ```xml
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency> 
    ```

4. Change the JNDI resource in `jetty-env.xml` to match your MySQL installation. For example, it might look like this:

    ```xml
    <New id="webDS" class="org.eclipse.jetty.plus.jndi.Resource">
        <Arg>jdbc/web</Arg>
        <Arg>
            <New class="org.apache.commons.dbcp.BasicDataSource">
                <Set name="driverClassName">com.mysql.jdbc.Driver</Set>
                <Set name="url">jdbc:mysql://localhost:3306/broadleaf?useUnicode=true&amp;characterEncoding=utf8</Set>
                <Set name="username">root</Set>
                <Set name="password"></Set>
            </New>
        </Arg>
    </New>

    <New id="webSecureDS" class="org.eclipse.jetty.plus.jndi.Resource">
        <Arg>jdbc/secure</Arg>
        <Arg>
            <New class="org.apache.commons.dbcp.BasicDataSource">
                <Set name="driverClassName">com.mysql.jdbc.Driver</Set>
                <Set name="url">jdbc:mysql://localhost:3306/broadleaf?useUnicode=true&amp;characterEncoding=utf8</Set>
                <Set name="username">root</Set>
                <Set name="password"></Set>
            </New>
        </Arg>
    </New>

    <New id="webStorageDS" class="org.eclipse.jetty.plus.jndi.Resource">
        <Arg>jdbc/storage</Arg>
        <Arg>
            <New class="org.apache.commons.dbcp.BasicDataSource">
                <Set name="driverClassName">com.mysql.jdbc.Driver</Set>
                <Set name="url">jdbc:mysql://localhost:3306/broadleaf?useUnicode=true&amp;characterEncoding=utf8</Set>
                <Set name="username">root</Set>
                <Set name="password"></Set>
            </New>
        </Arg>
    </New>
    ```
    > Note: You must change `jetty-env.xml` in both the `site` and the `admin` projects.
    > Note: I named my database `broadleaf`, make sure to use your database name in the url.

5. Update the runtime properties to use the correct MySQL dialect. In `core/src/main/resources/runtime-properties/common-shared.properties`, you will want to update the three persistence unit dialects to say:

    ```ini
    blPU.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    blSecurePU.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    blCMSStorage.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    ```

6. Update the build properties to use the correct MySQL dialect. In `build.properties`, you will want to update the following:

    ```ini
    ant.hibernate.sql.ddl.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    ...
    ant.blPU.url=jdbc:mysql://localhost:3306/broadleaf
    ant.blPU.userName=root
    ant.blPU.password=
    ant.blPU.driverClassName=com.mysql.jdbc.Driver

    ant.blSecurePU.url=jdbc:mysql://localhost:3306/broadleaf
    ant.blSecurePU.userName=root
    ant.blSecurePU.password=
    ant.blSecurePU.driverClassName=com.mysql.jdbc.Driver

    ant.blCMSStorage.url=jdbc:mysql://localhost:3306/broadleaf
    ant.blCMSStorage.userName=root
    ant.blCMSStorage.password=
    ```
And that's it! You should now be up and running with MySQL.

