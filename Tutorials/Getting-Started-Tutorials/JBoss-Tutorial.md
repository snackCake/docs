Our reference architecture specifies Tomcat as our default container. However, developers may wish to launch Broadleaf Commerce in JBoss for any
number of reasons. This tutorial provides the steps necessary to use JBoss AS 7 as the container in which to launch an instance of Broadleaf Commerce version 3.1.x.
Note, JBoss AS 7 compatibility for Broadleaf Commerce requires the latest snapshot of 3.1.1. 3.1.1-GA (when released) will also be compatible with JBoss AS 7.

Assumptions:
1. You're using 3.1.1-SNAPSHOT (or 3.1.1-GA, when available)
2. You're using MySql (you can swap out the instructions below with the driver and JDBC information applicable to your RDBMS)
3. You're launching JBoss using the standalone configuration
4. You're installing the Broadleaf Commerce Heat Clinic demo on JBoss (or a project based on the Heat Clinic demo)

Configure JBoss

1. Download and install JBoss AS 7 (https://www.jboss.org/jbossas/downloads.html)

2. Setup datasources for Broadleaf Commerce.

    a. Edit standalone.xml ([jboss installation]/standalone/configuration/standalone.xml)
    b. Add datasource definitions to the datasources subsystem element (change information as appropriate for your environment)

    ```xml
    <subsystem xmlns="urn:jboss:domain:datasources:1.0">
        <datasources>
            ...
            <datasource jndi-name="java:/jdbc/web" pool-name="webDS" enabled="true" use-java-context="true">
                <connection-url>jdbc:mysql://localhost:3306/broadleaf</connection-url>
                <driver>mysql</driver>
                <security>
                    <user-name>root</user-name>
                    <password>sa</password>
                </security>
            </datasource>
            <datasource jndi-name="java:/jdbc/storage" pool-name="storageDS" enabled="true" use-java-context="true">
                <connection-url>jdbc:mysql://localhost:3306/broadleaf</connection-url>
                <driver>mysql</driver>
                <security>
                    <user-name>root</user-name>
                    <password>sa</password>
                </security>
            </datasource>
            <datasource jndi-name="java:/jdbc/secure" pool-name="secureDS" enabled="true" use-java-context="true">
                <connection-url>jdbc:mysql://localhost:3306/broadleaf</connection-url>
                <driver>mysql</driver>
                <security>
                    <user-name>root</user-name>
                    <password>sa</password>
                </security>
            </datasource>
            ...
    ```

3. Setup a database driver

    a. Edit standalone.xml
    b. Add a driver definition to the datasources subsystem element
    ```xml
    <subsystem xmlns="urn:jboss:domain:datasources:1.0">
        <datasources>
            ...
            <drivers>
                ...
                <driver name="mysql" module="com.mysql.jdbc">
                    <driver-class>com.mysql.jdbc.Driver</driver-class>
                </driver>
            </drivers>
        </datasources>
    </subsystem>
    ```

4. Install the driver jar file

    a. Create a new directory structure for your driver in your JBoss installation ([jboss installation]/modules/com/mysql/jdbc/main)
    b. Copy your JDBC driver jar into the "main" directory you just created
    c. Create a module.xml file in the "main" directory. Here are its contents:
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <module xmlns="urn:jboss:module:1.0" name="com.mysql.jdbc">
        <resources>
            <resource-root path="mysql-connector-java-5.1.25.jar"/>
            <!-- Insert resources here -->
        </resources>
        <dependencies>
            <module name="javax.api"/>
            <module name="javax.transaction.api"/>
        </dependencies>
    </module>
    ```

5. Configure SSL (allows secure pages during checkout in Heat Clinic)

    a. A `mvn install` on the Heat Clinic demo will cause a keystore file to be created in [heat clinic installation]/site/target/[module name]/WEB-INF/blc-example.keystore
    b. You can use this sample keystore file to configure SSL for JBoss (or, you can use a keystore from a real certificate)
    c. Copy blc-example.keystore to [jboss installation]/standalone/configuration/blc-example.keystore
    d. Edit standalone.xml
    e. Add a secure connector to the web subsystem element
    ```xml
    <subsystem xmlns="urn:jboss:domain:web:1.1" native="false" default-virtual-server="default-host">
        ...
        <connector name="https" protocol="HTTP/1.1" scheme="https" socket-binding="https" secure="true">
            <ssl name="https" password="broadleaf" certificate-key-file="../standalone/configuration/blc-example.keystore"/>
        </connector>
        ...
    </subsystem>
    ```

Configure the Application

1. It's best to avoid the JPA scanning done by JBoss. This can be achieved by making sure there are no files named persistence.xml in the codebase.

    a. In older versions of Heat Clinic, there is a filed called persistence.xml in [heat clinic installation]/core/src/main/resources/META-INF/persistence.xml
    b. If it's not done already, rename this file persistence-core.xml
    c. Change any spring application context elements pointing to this file to persistence-core.xml (see this example)
    ```xml
    <bean id="blMergedPersistenceXmlLocations" class="org.springframework.beans.factory.config.ListFactoryBean">
        <property name="sourceList">
            <list>
                <value>classpath*:/META-INF/persistence-core.xml</value>
            </list>
        </property>
    </bean>
    ```
    d. Here is the list of known files containing this persistence.xml reference
        1. [heat clinic installation]/admin/src/main/webapp/WEB-INF/applicationContext-admin.xml
        2. [heat clinic installation]/site/src/main/webapp/WEB-INF/applicationContext.xml

2. Remove any unused resource-ref elements in web.xml

    a. In regard to Heat Clinic, if it's not removed already, delete the resource-ref element from [heat clinic installation]/site/src/main/webapp/WEB-INF/web.xml

3. Configure the application to exclude some unwanted JBoss modules

    a. Create a file called jboss-deployment-structure.xml at
        1. [heat clinic installation]/admin/src/main/webapp/WEB-INF/jboss-deployment-structure.xml
        2. [heat clinic installation]/admin/src/main/webapp/WEB-INF/jboss-deployment-structure.xml
    b. The contents of the file are:
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <jboss-deployment-structure>
        <deployment>
            <exclusions>
               <module name="org.hibernate"/>
               <module name="org.javassist"/>
               <module name="org.apache.log4j" />
            </exclusions>
            <dependencies>
                <module name="org.jboss.ironjacamar.jdbcadapters" slot="main"/>
            </dependencies>
        </deployment>
    </jboss-deployment-structure>
    ```

4. Update the runtime properties to use the correct MySQL dialect.

    a. In [heat clinic installation]/core/src/main/resources/runtime-properties/common-shared.properties, you will want to update the three persistence unit dialects to say:
    ```text
    blPU.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    blSecurePU.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    blCMSStorage.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    ```

5. (Optional) Make the site module deploy as ROOT.war (rather than mycompany.war)

    a. This allows you to hit the Heat Clinic home page at http://localhost:8080 (rather than http://localhost:8080/mycompany)
    b. Disable the default JBoss application
        1. Edit standalone.xml
        2. Change the enable-welcome-root param to false
        ```xml
        <virtual-server name="default-host" enable-welcome-root="false">
        ```
    c. Change site's generated war file name
        1. Edit [heat clinic installation]/site/pom.xml
        2. Change the finalName element value to `ROOT`
        ```xml
        ...
        <build>
            <outputDirectory>${webappDirectory}/WEB-INF/classes</outputDirectory>
            <finalName>ROOT</finalName>
            <plugins>
                <plugin>
        ...
        ```

At this point, you can perform a `mvn install` on your project and take the generated war files for admin and site and place them in
[jboss installation]/standalone/deployments so that the JBoss deployment scanner can pick them up and perform the deployment.

