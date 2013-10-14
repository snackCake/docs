# Removing the CMS module

As of Broadleaf 3.1, it is now possible to run without loading the content-management module. The vast majority of applications will want to continue using this module. However, a minority of implementations may want to remove this module if they are relying on a third party solution. These steps explain how to remove the content-management module from a normal DemoSite.

## Application steps

1. Exclude the contentmanagement module from your pom.xml

2. Remove the jndi info for `jdbc/storage` in both the site and admin `applicationContext-datasource.xml`

3. Remove the `blCMSStorage` bean from core/persistence.xml

4. Remove `PageController` from the site project

5. Remove the `blURLHandlerFilter` definition from `applicationContext-filter.xml`

6. Add the following bean to your site `applicationContext-servlet.xml` file:

    ```xml
    <bean id="blConfiguration" class="org.broadleafcommerce.common.config.RuntimeEnvironmentPropertiesConfigurer"/>
    ```

7. In the same file, remove the `org.broadleafcommerce.cms.web.PageHandlerMapping` bean

8. Remove the `webStorageDS` map entry from `blMergedDataSources` in both site's `applicationContext.xml` and admin's `applicationContext-admin.xml`

9. Remove the `blStaticMapNamedOperationComponent` bean from site's `applicationContext.xml`

9. In your site `web.xml`, remove the following two lines from `patchConfigLocation`:

    ```xml
    classpath:/bl-open-admin-contentClient-applicationContext.xml
    classpath:/bl-cms-contentClient-applicationContext.xml
    ```

    and remove the following line from `contextConfigLocation`:

    ```xml
    classpath:/applicationContext-servlet-cms-contentClient.xml
    ```

## Data steps

If you are not using Hibernate's DDL create option, you won't need to worry about this. Otherwise, you will need to remove anything that references CMS specific data from these scripts.


  
