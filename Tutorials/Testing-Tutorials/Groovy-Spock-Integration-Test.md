##Spock/Groovy Integration Tests

With the 4.1 release of Spring, we now have the ability to test RESTful services and other integration test abilities in a much simpler format. With a few annotations, we can quickly write integration tests that utilize a Spring generated application context.

###Pom Setup

To use this new method of integration test creation, you will need to modify your pom with the following dependencies and plugins.

####Dependencies

Before we move into the test class's configuration, we need to include some dependencies in the project's pom.xml. Please make sure you have the following:
```xml
...    
<repositories>
    <repository>
        <id>maven central snapshots</id>
        <name>maven central snapshots</name>
        <url>https://oss.sonatype.org/content/repositories/snapshots</url>
    </repository>
</repositories>

...

<dependency>
  <groupId>org.broadleafcommerce</groupId>
  <artifactId>integration</artifactId>
  <version>3.2.0-SNAPSHOT</version>
  <type>jar</type>
  <classifier>tests</classifier>
  <scope>test</scope>
  <optional>true</optional>
</dependency>
<dependency>
  <groupId>org.broadleafcommerce</groupId>
  <artifactId>integration</artifactId>
  <version>3.2.0-SNAPSHOT</version>
  <type>jar</type>
  <scope>test</scope>
  <optional>true</optional>
</dependency>
<dependency>
  <groupId>org.codehaus.groovy</groupId>
  <artifactId>groovy-all</artifactId>
  <version>2.1.8</version>
  <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.spockframework</groupId>
    <artifactId>spock-spring</artifactId>
    <version>1.0-groovy-2.0-SNAPSHOT</version>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.codehaus.groovy</groupId>
            <artifactId>groovy-all</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>3.0.1</version>
  <scope>test</scope>
</dependency>
```
###Plugins

With the dependencies, we want to use some new plugins to help with testing in general. These are the Surefire and Failsafe plugins. They have the following pom entries:

```xml
...
<build>
  ...
  <plugins>
  ...
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>2.10</version>
        <configuration>
            <includes>
                <include>**/*Spec*</include>
            </includes>
            <excludes>
                <exclude>**/browsertests/**</exclude>
                <exclude>**/*IT*</exclude>
            </excludes>
            <properties>
              ...
            </properties>
        </configuration>
    </plugin>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-failsafe-plugin</artifactId>
        <version>2.17</version>
        <executions>
            <execution>
                <goals>
                    <goal>integration-test</goal>
                    <goal>verify</goal>
                </goals>
            </execution>
        </executions>
        <configuration>
            <includes>
                <include>**/*IT*</include>
            </includes>
        </configuration>
    </plugin>
    ...
  </plugins>
...
</build>
```

>Make sure you have the GMaven plugin set up or else it won't recognize the groovy files.

With these plugins, we can now do the following:

* When we run `mvn clean install` it will run all both the unit tests specified by the Surefire plugin, as well as the the integration tests (which must include IT in their name) specified by the Failsafe plugin.
* If you do not want to include running the integration tests, use `mvn clean install -DskipITs` to have it just run the unit tests and ignore the integration tests. You may want to use this when you are just testing your unit tests during their creation and don't want to run the longer integration tests.

###Test Spec Setup

We are now using a new format for the Spock/Groovy tests. We now have an idea of an `IntegrationSetup` file and an `ITSpec` file.

####IntegrationSetup

`IntegrationSetup` files extend from base setup files on a per project basis and add all of the application Contexts that are required by the project. Every Project's `IntegrationSetup` file will extend from either `AdminIntegrationSetup` or `SiteIntegrationSetup` depending upon which kind of integration test you are attempting. For example, in the CustomField module, we created an integration test that interacts on the admin side, so we create a `CustomFieldAdminIntegrationSetup` file as follows:

```groovy
package com.broadleafcommerce.customfield.spec
...

@ContextHierarchy([
@ContextConfiguration(name = "adminRoot", locations = [
    "classpath:/bl-custom-field-admin-applicationContext.xml",
    "classpath:/bl-custom-field-applicationContext.xml"])])
class CustomFieldAdminIntegrationSetup extends RootAdminIntegrationSetup {

}
```

Note that there is no code in the body of this class, and it does not specify a loader, or other annotations. Only a `@ContextHierarchy` with a single `@ContextConfiguration` that references the `adminRoot` ContextConfiguration of the `AdminIntegrationSetup` file, and includes the Contexts that the CustomField Module requires for admin Integration Tests. If we needed to create a setup file for Site based tests, we could call it `CustomFieldSiteIntegrationSetup`, extend from the `SiteIntegrationSetup` and reference the `siteRoot` ContextConfiguration.

####ITSpec

After we have created the `IntegrationSetup` files, we can now create `ITSpec` files that have actual test code inside them.
>The class names of these specs must follow the format of either `*AdminITSpec` or `*SiteITSpec`.

These classes extend from the setup file you created earlier for your project's Integration Tests, they have no class level annotations, and just include test code. As an example, here is an exerpt from `CustomFieldAdminITSpec`

```groovy
package com.broadleafcommerce.customfield.spec

import ...

class CustomFieldAdminITSpec extends CustomFieldAdminIntegrationSetup {

  @Resource
  RuleBuilderFieldServiceFactory ruleBuilderFieldServiceFactory
...

```

The further contents of this file include normal groovy syntax based tests.

###Testing RESTful Services

With this new integration test setup, we can now utilize the Spring-Test's `MockMVC` api to test RESTful services. To do so, along with the above annotations, you will need to include a few new lines.

```groovy
...
class testClassITSpec extends Specification {

@Resource
WebAppContext webAppContext

MockMvc mockMvc

def setup() {
    mockMVC = MockMVCBuilders.webAppContextSetup(this.webAppContext).build()
}

...

```

These lines populate the `WebAppContext` with the one created by Spring's `@ContextConfiguration` annotation, declare our `MockMVC` object, and then instantiate it with the `WebAppContext`. Now, with it having been created, we can call restful services using MockMvc's only method, `perform()`, such as:

```groovy
def "Test RESTful service example"() {

...

mockMvc.perform(post("/restful/service/example")
    .content(standardOrderJSON())
    .contentType(MediaType.APPLICATION_JSON)
    .accept(MediaType.APPLICATION_JSON))
  .andDo(print())
  .andExpect(status().isCreated())

mockMvc.perform(get("restful/service/example")
    .accept(MediaType.APPLICATION_JSON))
  .andDo(print())
  .andExpect(status().isOk())
  .andExpect(jsonPath("$[0].items['" + RestDataFixture.YUMMY_ITEM + "']").value(12))
  
  ...
  
  }

```

