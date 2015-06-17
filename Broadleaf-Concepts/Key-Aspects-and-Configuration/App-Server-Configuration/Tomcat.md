# Tomcat

This is extremely similar to the current configuration in Jetty. In site/pom.xml you will see the following plugin defined for Jetty:

```xml
<plugin>
    <groupId>org.mortbay.jetty</groupId>
    <artifactId>maven-jetty-plugin</artifactId>
    <version>6.1.22</version>
    <configuration>
        <webAppSourceDirectory>${webappDirectory}</webAppSourceDirectory>
        <contextPath>/mycompany</contextPath>
        <stopPort>9966</stopPort>
        <stopKey>foo</stopKey>
        <connectors>
            <connector implementation="org.mortbay.jetty.nio.SelectChannelConnector">
                <port>8080</port>
                <maxIdleTime>60000</maxIdleTime>
            </connector>
        </connectors>
    </configuration>
</plugin>
```

You can either replace this plugin or just add an additional plugin below:

```xml
<plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <!-- for Tomcat 7, change to tomcat7-maven-plugin -->
    <artifactId>tomcat6-maven-plugin</artifactId>
    <version>2.0-beta-1</version>
    <configuration>
        <path>/mycompany</path>
        <warSourceDirectory>${webappDirectory}</warSourceDirectory>
        <port>8080</port>
    </configuration>
</plugin>
```

You can then optionally modify build.xml to add a new ant task for this:

```xml
<target name="tomcat-demo" depends="start-db">
    <delete dir="war/WEB-INF/lib"/>
    <artifact:mvn mavenHome="${maven.home}" fork="true" jvmargs="-DbroadleafCoreDirectory=${broadleafCoreDirectory} -DbroadleafWorkspaceDirectory=${broadleafWorkspaceDirectory} -XX:MaxPermSize=256M -Xmx512M">
        <arg value="compile"/>
        <arg value="war:exploded"/>
        <arg value="tomcat6:run-war"/>
    </artifact:mvn>
</target>
```

For admin, the configuration is similar but you would change the ```port``` to ```8081```. For more information on this plugin, check out the [plugin documentation](http://tomcat.apache.org/maven-plugin-2.2/run-mojo-features.html).

## Character Encoding

We will make a few configuration changes to enable `UTF-8` character encoding in Tomcat. 

Configure your **Resources** in `context.xml` to include `connectionProperties`. The format of the string will need to be in the `propertyName=property;` format. Here is a sample resource using MySql and UTF-8 encoding:

```xml
<Resource name="jdbc/web" auth="Container" type="javax.sql.DataSource"
               maxActive="30" maxIdle="60" maxWait="10000"
               username="username" password="password" driverClassName="com.mysql.jdbc.Driver"
               connectionProperties="useUnicode=true;characterEncoding=utf8;"
               url="jdbc:mysql://localhost/broadleaf"/>
```

Alternatively the `url` can be expanded to include the connection properties:

```xml
url="jdbc:mysql://localhost:3306/broadleaf?useUnicode=true&characterEncoding=utf8"
```

Configure your **Connectors** in `server.xml` with `URIEncoding="UTF-8"` to encode url (GET request) parameters. This ensures that Tomcat handles all incoming GET parameters as UTF-8 encoded. Here is sample Connector tag:

```xml
<Connector port="8080" protocol="HTTP/1.1"
              connectionTimeout="20000"
              redirectPort="8443" 
              URIEncoding="UTF-8"/>
```

> Note : You will need to set up your database to UTF-8 Collation as well.