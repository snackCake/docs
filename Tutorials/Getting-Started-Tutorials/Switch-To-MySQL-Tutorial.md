# Switch To MySQL Tutorial

A very common step that users may want to do is switch away from the bundled HSQL database to a more mature database such as MySQL. We have a general guide on [[using MySQL | MySQL]], but this tutorial will be more explicit specifically regarding Heat Clinic.

1. Download and install MySQL Database (http://dev.mysql.com/downloads/mysql/)

    > For proper UTF8 configuration and easy of use across multiple platforms (Linux, MySQL, Windows) we also recommend these minimum settings in `my.cnf`:

    ```ini
    [mysqld]
    lower_case_table_names=1
    character-set-server=utf8
    collation-server=utf8_general_ci
    ```


2. Create a new database and a user capable of accessing this database with privileges for creating tables included (see MySQL documentation if you have questions about how to administrate databases and users).


3. In your root `pom.xml`, find the following in the `<dependencies>` section under the `<plugin>` with `<groupId>org.apache.tomcat.maven</groupId>`

    ```xml
    <dependency>
        <groupId>org.hsqldb</groupId>
        <artifactId>hsqldb</artifactId>
        <version>2.3.1</version>
        <type>jar</type>
        <scope>compile</scope>
    </dependency>
    ```

    and replace it with

    ```xml
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.26</version>
    </dependency>
    ```


4. Update the runtime properties to use the correct MySQL dialect. In `core/src/main/resources/runtime-properties/common-shared.properties`, you will want to update the three persistence unit dialects to say:

    ```ini
    blPU.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    blSecurePU.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    blCMSStorage.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    ```


5. Update the build properties to use the correct MySQL database. In `build.properties`, you will want to update the following:

    ```ini
    database.user=sa
    database.password=
    database.driver=com.mysql.jdbc.Driver
    database.url=jdbc:mysql://localhost:3306/broadleaf?useUnicode=true&amp;characterEncoding=utf8
    ```
    > Note: `database.driver` and `database.url` have been changed from HSQLDB to MySQL.
    > Note: I named my database `broadleaf`, make sure you use your database name in the url.


6. Remove the HSQL dependency from the site ant targets. In your `site/build.xml` file replace

    ```xml
    <target name="tomcat" depends="start-db">
    ```

    with

    ```xml
    <target name="tomcat">
    ```

    and replace

    ```xml
    <target name="tomcat-jrebel" depends="start-db">
    ```

    with

    ```xml
    <target name="tomcat-jrebel">
    ```


And that's it! You should now be up and running with MySQL.

