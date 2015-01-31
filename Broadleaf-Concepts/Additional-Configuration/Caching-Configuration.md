# Caching Configuration

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

## Default Cache Configurations

Broadleaf provides a number of ehcache regions out of the box. These are defined in the following locations:

- [Common](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/90ef66abba898fbb70e8246d1ab194572d803a2b/common/src/main/resources/bl-common-ehcache.xml)
- [Profile](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/a4fa1e56d363860b566115b1c782f1b7e1a8fbba/core/broadleaf-profile/src/main/resources/bl-ehcache.xml)
- [CMS](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/919241ae50c7c201a41817b047aae69fb4fcc62d/admin/broadleaf-contentmanagement-module/src/main/resources/bl-cms-ehcache.xml)

## Additional Cache Regions with Enterprise

### Theme:

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

### Account Credit

```xml
<cache
    name="blCreditAccountElements"
    maxElementsInMemory="100"
    eternal="false"
    overflowToDisk="false"
    timeToLiveSeconds="1"
    timeToIdleSeconds="10"/>
```

### i18nEnterprise

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

### Price Lists

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

### Subscription

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

1. Add the following to an `applicationContext`, either `site` or `admin`:

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

2. Start up the server, and connect to it via a JMX listener like VisualVM. Assuming that you are running locally, VisualVM will display all local Java processes on the lefthand side that you can easily attach to. In this screenshot, I have attached to my locally running Broadleaf instance that I started via Ant/Maven on the command line:

![VisualVM](https://s3.amazonaws.com/f.cl.ly/items/000w0m1q1V1E2s1g0A1w/Screen%20Shot%202015-01-31%20at%2010.06.12%20AM.png)

If you go over to the MBeans tab, you will see the exports from the server. We are interested in the **net.sf.ehcache** folder and the **Cache -> __DEFAULT__** MBean:

![ehcachebean](https://s3.amazonaws.com/f.cl.ly/items/1u100o260g3y0k041E2S/Screen%20Shot%202015-01-31%20at%2010.10.58%20AM.png)

If you click on a specific MBean you can hit the `removeAll` button to clear the cache:

![removecache](https://s3.amazonaws.com/f.cl.ly/items/440z3j1Y011n3U0S2g2u/Screen%20Shot%202015-01-31%20at%2010.12.48%20AM.png)

// Queries
query.Offer
query.Order
query.ContentTest
query.Catalog
query.Cms


// Entity annotation caches and others
blConfigurationModuleElements
blCMSElements
blProducts
blOffers
blStandardElements
blOfferElements
blTranslationElements
blCategories
blOrderElements

blSandBoxElements

blCreditAccountElements

blThemeFiles

// Explicit Cache Stuff
blProductUrlCache - findProductByUrl
blCategoryUrlCache - findCategoryByUrl
blSystemPropertyElements - lookup system property from DB
blTranslationElements - translations from TranslationService
blBundleElements - resource bundling/minification
generatedResourceCache - admin-managed css files and theme files
cmsPageCache - findPageByURI
cmsPageMapCache - after resolving a page, doing a lookup to populate the properties of the page
cmsStructuredContentCache - structured content
cmsUrlHandlerCache - URL redirects
blTemplateElements - caching individual parts of a page
blInternationalMessageElements - i18n property translations
blThemeConfigurationUpdated - cache for bundles that happen when resolving files from the db




cache.page.templates
cache.page.templates.ttl
