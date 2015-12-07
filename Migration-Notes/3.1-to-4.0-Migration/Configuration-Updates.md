# Site Configuration Updates
### Configuration file updates

1. build.properties
  - [Download the Spring Instrument 4.1.6 jar](http://central.maven.org/maven2/org/springframework/spring-instrument/4.1.6.RELEASE/spring-instrument-4.1.6.RELEASE.jar) into your `lib` directory and replace `spring.instrument.path=../lib/spring-instrument-3.2.2.RELEASE.jar` with `spring.instrument.path=../lib/spring-instrument-4.1.6.RELEASE.jar`
2. Schema locations (in all `applicationContext.xml` files)
  - Replace all instances of `spring-beans-3.2.xsd` with `spring-beans-4.1.xsd`
  - Replace all instances of `spring-context-3.2.xsd` with `spring-context-4.1.xsd`
  - Replace all instances of `spring-security-3.1.xsd` with `spring-security-3.2.xsd`
  - Replace all instances of `spring-util-3.2.xsd` with `spring-util-4.1.xsd`
  - Replace all instances of `spring-jee-3.2.xsd` with `spring-jee-4.1.xsd`
  - Replace all instances of `spring-tx-3.2.xsd` with `spring-tx-4.1.xsd`
  - Replace all instances of `spring-aop-3.2.xsd` with `spring-aop-4.1.xsd`
  - Replace all instances of `spring-mvc-3.2.xsd` with `spring-mvc-4.1.xsd`
3. Thymeleaf
  - Change all references to `org.thymeleaf.spring3.*` to `org.thymeleaf.spring4.*`


###Changes to the root `pom.xml`
1. Add the following to the `<repositories>` tag to grab Broadleaf 4.0 and the Menu module:

    ```xml
    <repository>
      <id>public releases</id>
      <name>public releases</name>
      <url>http://nexus.broadleafcommerce.org/nexus/content/repositories/releases/</url>
    </repository>
    ```

2. Inside the `<properties>` tag, change `blc.version`'s value to `4.0.0-GA` and add `<blc.menu.version>1.0.0-GA</blc.menu.version>`
  
3. Change the version of `maven-compiler-plugin` to 3.1 (the latest at time of writing)
4. Add

    ```xml
    <dependency>
      <groupId>org.broadleafcommerce</groupId>
      <artifactId>broadleaf-menu</artifactId>
      <version>${blc.menu.version}</version>
      <type>jar</type>
      <scope>compile</scope>
    </dependency>
    ```
    to `<dependencies>`
  

### Module application context registration

As of Broadleaf 4.0, each BLC module's application contexts do not need to be manually listed in your project's `admin/web.xml` or `site/web.xml`. Instead, the application context registration will be done using the following lines:

1. Include the necessary `patchConfigLocation` file for the admin servlet in your `admin/web.xml`:
    `classpath*:/blc-config/admin/bl-*-applicationContext.xml`

2. Include the necessary `contextConfigLocation` file for the admin servlet in your `admin/web.xml`:
    `classpath*:/blc-config/admin/bl-*-admin-applicationContext-servlet.xml`

3. Include the necessary `patchConfigLocation` file in your `site/web.xml`:
    `classpath*:/blc-config/site/bl-*-applicationContext.xml`

4. Include the necessary `contextConfigLocation` file in your `site/web.xml`:
    `classpath*:/blc-config/site/bl-*-applicationContext-servlet.xml`

###Other changes to web.xml (site and admin)
1. Replace

    ```xml
    <web-app version="2.4" xmlns="http://java.sun.com/xml/ns/j2ee"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
      http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd" id="WebApp_ID">
    ```
  
    with
  
    ```xml
    <web-app
      version="3.0"
      xmlns="http://java.sun.com/xml/ns/javaee"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
      http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">
    ```
    
2. Insert
  
    ```xml
    <absolute-ordering />
    ```
  after
  
    ```xml
      ...
    </context-param>
    ```

3. Replace
  
    ```xml
    <listener-class>org.broadleafcommerce.common.web.extensibility.MergeContextLoaderListener</listener-class>
    ```
    with
  
    ```xml
    <listener-class>org.broadleafcommerce.common.web.extensibility.MergeContextLoader</listener-class>
    ```


###Changes to web.xml (site only)
1. Replace

    ```xml
    <param-value>com.fasterxml.jackson.jaxrs</param-value>
    ```
    with
    
    ```xml
    <param-value>
      com.fasterxml.jackson.jaxrs.json
    </param-value>
    ```