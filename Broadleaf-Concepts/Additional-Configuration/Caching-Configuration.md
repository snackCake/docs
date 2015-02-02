## Caching Configuration

Configuring cache settings follows the same pattern as other configuration in Broadleaf.

You will want to define a bean in your `applicationContext.xml` file that specifies additional config locations for Broadleaf to merge. This bean will look similar to this:

```xml
<bean id="blMergedCacheConfigLocations" class="org.springframework.beans.factory.config.ListFactoryBean">
    <property name="sourceList">
        <list>
            <value>classpath:bl-override-ehcache.xml</value>
        </list>
    </property>
</bean>
```

Broadleaf will then pick up this definition and merge it with the default configuration, overriding any regions that Broadleaf has defined while creating new ones.

> Note: if you have started using Broadleaf with the DemoSite "Heat Clinic" project then this is already configured for you

## Caching with Enterprise

With the Broadleaf Enterprise license caches are expired whenever you make edits in the admin panel. This allows you to set caches extremely 

## Default Cache Configurations

Broadleaf provides a number of ehcache regions out of the box with sensible defaults. These are defined in the following locations:

- [Common](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/90ef66abba898fbb70e8246d1ab194572d803a2b/common/src/main/resources/bl-common-ehcache.xml)
- [Profile](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/a4fa1e56d363860b566115b1c782f1b7e1a8fbba/core/broadleaf-profile/src/main/resources/bl-ehcache.xml)
- [CMS](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/919241ae50c7c201a41817b047aae69fb4fcc62d/admin/broadleaf-contentmanagement-module/src/main/resources/bl-cms-ehcache.xml)

These default settings were determined empirically using load testing results from [our scalability white paper](http://www.broadleafcommerce.com/scalability).

### Additional Cache Regions with Enterprise

Beyond the above default cache configurations, various modules that are included with the Enterprise license define new regions. These are notated below.

#### Theme:

```xml
<!-- 1 hour cache -->
<cache
    name="blThemeConfigurationUpdated"
    maxElementsInMemory="100"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="3600"/>

<!-- 1 hour cache -->
<cache
    name="blThemeFiles"
    maxElementsInMemory="10000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="3600"/>

<cache name="blThemeFileNullCheckCache"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="3600"/>
```

#### Account Credit

```xml
<cache
    name="blCreditAccountElements"
    maxElementsInMemory="100"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="1"
    timeToIdleSeconds="10"/>
```

#### i18nEnterprise

```xml
<cache
    name="blInternationalMessageElements"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="600"/>

<cache name="blInternationalMessageNullCheckCache"
   maxElementsInMemory="1000"
   eternal="false"
   overflowToDisk="false"
   timeToLiveSeconds="600"/>
```

#### Price Lists

```xml
<diskStore path="java.io.tmpdir"/>

<!-- 1 hour cache -->
<cache
    name="blPriceListCache"
    maxElementsInMemory="100"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="3600"/>
```

#### Subscription

```xml
<!-- 1 hour cache -->
<cache
    name="blSubscriptionCache"
    maxElementsInMemory="100"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="3600"/>
```

## Manually Expiring Caches in a Running Application

The below methodology exposes the `EHCache` regions over JMX as MBeans for control. You can then connect to the Java process via a JMX console like [VisualVM](http://visualvm.java.net/download.html).

One drawback with this is that you will get duplicate MBean exceptions if you are running the `site` and `admin` on the same JVM (deploying to the same Tomcat instance).

1. Add the following to an `applicationContext` (either `site` or `admin`):

    ```xml
    <bean id="mbeanServer" class="org.springframework.jmx.support.MBeanServerFactoryBean">
        <!-- indicate to first look for a server -->
        <property name="locateExistingServerIfPossible" value="true"/>
    </bean>
    
    <bean id="managementService"
        class="net.sf.ehcache.management.ManagementService"
        init-method="init"
        destroy-method="dispose">
        <constructor-arg ref="blCacheManager"/>
        <constructor-arg ref="mbeanServer"/>
        <constructor-arg index="2" value="true"/>
        <constructor-arg index="3" value="true"/>
        <constructor-arg index="4" value="true"/>
        <constructor-arg index="5" value="true"/>
    </bean>
    ```

2. Start up the server, and connect to it via a JMX listener like VisualVM. If you are running locally, VisualVM will display all local Java processes on the lefthand side that you can easily attach to. In this screenshot, I have attached to my locally running Broadleaf instance that I started via Ant/Maven on the command line:

    ![visualvm-local](caching/visual-vm-local.png)

    If you go over to the MBeans tab, you will see the exports from the server. We are interested in the **net.sf.ehcache** folder and the **Cache -> __DEFAULT__** MBean:

    ![visual-vm-ehcache](caching/visual-vm-ehcache-mbean.png)

    If you click on a specific MBean you can hit the `removeAll` button to clear the cache:

    ![visual-vm-cmscache](caching/visual-vm-cmscache.png)

### Simple Remote JMX Connections

Add the following arguments to your JVM (if you are using Tomcat, this is `CATALINA_OPTS`):

```console
-Dcom.sun.management.jmxremote 
-Dcom.sun.management.jmxremote.port=4446 
-Dcom.sun.management.jmxremote.authenticate=false 
-Dcom.sun.management.jmxremote.ssl=false 
```

This opens port 4446 for remote connection

#### Remote JMX Connections Securely behind a Firewall

The above configuration assigns a random external port for RMI which makes it impossible to securely connect to the running application and only whitelist a specific port. The below instructions assume Tomcat, but there are likely equivalents on other application servers.

1. In `server.xml`, add this line anywhere in the `<Server>` tag, replacing the placeholder ports. The `rmiRegistryPortPlatfor` is the JMX remote port to connect to while the `rmiServerPortPlatform` is internally used by Tomcat.

    ```xml
    <Listener className="org.apache.catalina.mbeans.JmxRemoteLifecycleListener" rmiRegistryPortPlatform="30000" rmiServerPortPlatform="30001"/>
    ```

2. Create a `jmxremote.access` if one does not already exist 

    ```console
    controlRole   readwrite \
                  create javax.management.monitor.*,javax.management.timer.* \
                  unregister
    ```
    
3. Create a `jmxremote.access` if one does not already exist **with a chmod of 0600**

    ```console
    controlRole  somepassword
    ```

4. Change the system properties above to authenticate and specify the password and access file.

    ```console
    -Dcom.sun.management.jmxremote.ssl=false
    -Dcom.sun.management.jmxremote.authenticate=true
    -Dcom.sun.management.jmxremote.password.file=/etc/java-7-openjdk/management/jmxremote.password
    -Dcom.sun.management.jmxremote.access.file=/etc/java-7-openjdk/management/jmxremote.access
    -Djava.rmi.server.hostname=1.2.3.4
    ```
    
    > Note: you could also use `com.sun.management.jmxremote.ssl=true` and use the domain name for the hostname if you have a valid SSL certificate
    
5. In VisualVM right click on "Remote" and hit "Add Remote Host..."

    ![remote host](caching/visual-vm-remotehost.png)

6. Select the host that you just added and go to "Add JMX Connection"

    ![add JMX remote](caching/visual-vm-jmx.png)


## Disabling the Cache

You can also add configuration to completely disable all caching in Broadleaf. If you started from the DemoSite Heat Clinic starter project you should have a `bl-override-ehcache.xml` file in the `site` project (in the admin, this is `bl-override-ehcache-admin.xml`). If not, the below changes should be in the file that you configured at the top of this document.

This sets all of the caches in Broadleaf to expire after 10 seconds. This might be useful in a development environment but **should not be checked into version control**.

```xml
<cache
    name="blTranslationElements"
    maxElementsInMemory="10000000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10"/>
    
<cache
    name="blConfigurationModuleElements"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>      

<cache
    name="blSystemPropertyElements"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>

<cache name="blSystemPropertyNullCheckCache"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>
    
<cache
    name="blBundleElements"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>

<diskStore path="java.io.tmpdir"/>

<defaultCache
    maxElementsInMemory="100000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10"
    timeToIdleSeconds="30"/>

<cache
    name="blStandardElements"
    maxElementsInMemory="100000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10">
    <cacheEventListenerFactory class="org.broadleafcommerce.common.cache.engine.HydratedCacheEventListenerFactory"/>
</cache>

<cache
    name="blProducts"
    maxElementsInMemory="100000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10">
    <cacheEventListenerFactory class="org.broadleafcommerce.common.cache.engine.HydratedCacheEventListenerFactory"/>
</cache>

<cache name="blProductUrlCache"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>

<cache
    name="blCategories"
    maxElementsInMemory="100000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10">
    <cacheEventListenerFactory class="org.broadleafcommerce.common.cache.engine.HydratedCacheEventListenerFactory"/>
</cache>

<cache name="blCategoryUrlCache"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>

<cache
    name="blOffers"
    maxElementsInMemory="100000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10">
    <cacheEventListenerFactory class="org.broadleafcommerce.common.cache.engine.HydratedCacheEventListenerFactory"/>
</cache>

<cache
    name="blInventoryElements"
    maxElementsInMemory="100000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10"/>
    
<cache
    name="org.hibernate.cache.internal.StandardQueryCache"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>

<cache
    name="query.Catalog"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>
    
<cache
    name="query.Cms"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>

<cache
    name="query.Offer"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>

<cache
    name="blOrderElements"
    maxElementsInMemory="100000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10"/>
    
 <cache
    name="blCustomerElements"
    maxElementsInMemory="100000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10"/>

<cache
    name="query.Order"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>
    
 <cache
    name="generatedResourceCache"
    maxElementsInMemory="100"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10"/>
    
 <!-- This is required by Hibernate to ensure that query caches return 
      corrrect results. It must contain at least as many entries as there are 
      DB tables. -->
 <cache name="org.hibernate.cache.spi.UpdateTimestampsCache" 
    maxElementsInMemory="5000" 
    eternal="true" 
    overflowToDisk="true"/>

<cache name="blTemplateElements"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10"/>

<cache
    name="blCMSElements"
    maxElementsInMemory="10000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10"/>

<cache name="cmsPageCache"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10"/>

<cache name="cmsStructuredContentCache"
    maxElementsInMemory="5000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10"/>

<!--  URLHandlerCache -->
<cache name="cmsUrlHandlerCache"
    maxElementsInMemory="5000"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10"/>
    
    
<cache
    name="blThemeConfigurationUpdated"
    maxElementsInMemory="100"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>

    <!-- 1 hour cache -->
<cache
    name="blThemeFiles"
    maxElementsInMemory="10000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>

<cache name="blThemeFileNullCheckCache"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>

<cache
    name="blCreditAccountElements"
    maxElementsInMemory="100"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"
    timeToIdleSeconds="10"/>

<cache
    name="blInternationalMessageElements"
    maxElementsInMemory="1000"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="10"/>

<cache name="blInternationalMessageNullCheckCache"
   maxElementsInMemory="1000"
   eternal="false"
   overflowToDisk="false"
   timeToLiveSeconds="10"/>
   
<cache
    name="blPriceListCache"
    maxElementsInMemory="100"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10"/>

<cache
    name="blSubscriptionCache"
    maxElementsInMemory="100"
    eternal="false"
    overflowToDisk="true"
    timeToLiveSeconds="10"/>
```

## Other cache settings

Broadleaf also uses various other caching configured by system properties. Add this to your environment-specific properties files to change the behavior.

```properties
cache.page.templates
cache.page.templates.ttl
```
