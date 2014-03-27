# Merge Configuration

Merge configuration is handled through your web.xml file. The merge facility in Broadleaf primarily operates by intelligently mixing one or more Spring application context files. The final merged version of the application context is then passed to Spring for processing. 

There are two different types of applicationContext files that exist in a typical Broadleaf application: core Spring files listed in the `patchConfigLocation` context param in web.xml, and Spring MVC files listed in the `contextConfigLocation` servlet init param in web.xml. The merge behavior of these two types of files is slightly different.

## Merging Core Spring Application Contexts

Broadleaf is capable of intelligently merging beans by providing a specialized `MergeContextLoader`, which is invoked as a listener by the following configuration in web.xml:

```xml
<listener>
    <listener-class>org.broadleafcommerce.common.web.extensibility.MergeContextLoaderListener</listener-class>
</listener>
```

This ContextLoader will analyze the contents of all applicationContexts specified in the `patchConfigLocation` and do its best to merge properties using two strategies. 

### Strategy 1: Set/List/Map FactoryBeans

This is the newer of the two strategies in Broadleaf, and all new configuration is written in this pattern. The easiest way to show this strategy is by example. Let's take a look at some configuration that lives in a Broadleaf applicationContext file:

```xml
<bean id="blDialectProcessors" class="org.springframework.beans.factory.config.SetFactoryBean">
    <property name="sourceSet">
        <set>
            <ref bean="blAddSortLinkProcessor" />
            <ref bean="blCategoriesProcessor" />
            ... other bean references ...
        </set>
    </property>
</bean>
<bean id="blDialect" class="org.broadleafcommerce.common.web.dialect.BLCDialect">
    <property name="processors" ref="blDialectProcessors" />
</bean> 
```

What we have here is a bean called `blDialect` that has a property in it called `processors` which takes a Set of things that implement the Thymeleaf `IProcessor`. This applicationContext file also defines another bean called `blDialectProcessors` of type `SetFactoryBean`. This means that Spring will do some processing for this SetFactoryBean, and when it is injected into the `processors` property, it will be transformed into a Set.

We are able to contribute an additional processor to this Set without copy-pasting the entire `blDialectProcessors` bean. We do so with the following configuration:

```xml
<bean id="blDialectAdditionalProcessors" class="org.springframework.beans.factory.config.SetFactoryBean">
    <property name="sourceSet">
        <set>
            <ref bean="blAdditionalProcessorOne"/>                
            <ref bean="blAdditionalProcessorTwo"/>                
        </set>
    </property>
</bean>
<bean class="org.broadleafcommerce.common.extensibility.context.merge.LateStageMergeBeanPostProcessor">
    <property name="collectionRef" value="blDialectAdditionalProcessors" />
    <property name="targetRef" value="blDialectProcessors" />
</bean>    
```

This additional configuration triggers the creation of a new bean, `blDialectAdditionalProcessors` that then defines two extra processors that we want to add to the `blDialect processors` property. Secondly, we define a `LateStageMergeBeanPostProcessor` that will merge our new bean into the existing bean. Then, when the existing bean is injected into `blDialect`, it will contain our extra entries.

This strategy is applicable for adding entries into any pre-defined Broadleaf beans.

### Strategy 2: XPath based merging

This older merge strategy is still used in parts of the system that have not been migrated to the new style. It leverages various XPath based merge strategies defined in [default.properties](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/master/common/src/main/resources/org/broadleafcommerce/common/extensibility/context/merge/default.properties). This approach actually modifies the XML structure of the merged applicationContexts before it reaches Spring. Let's take a look at an example of registering multiple dialects in the `blWebTemplateEngine`:

```xml
<bean id="blWebTemplateEngine" class="org.thymeleaf.spring3.SpringTemplateEngine">
    <property name="dialects">
        <set>
            <ref bean="thymeleafSpringStandardDialect" />
            <ref bean="blDialect" />
        </set>
    </property>
</bean> 
```

If we wanted to add our own custom dialect, we could simply place the following bean definition in our applicationContext file:

```xml
<bean id="blWebTemplateEngine" class="org.thymeleaf.spring3.SpringTemplateEngine">
    <property name="dialects">
        <set>
            <ref bean="myCustomDialect" />
        </set>
    </property>
</bean> 
```

Because of this chunk in default.properties:

```text
handler.14=org.broadleafcommerce.common.extensibility.context.merge.handlers.NodeReplaceInsert
priority.14=14
xpath.14=/beans/bean[@id='blWebTemplateEngine']/*
handler.14.1=org.broadleafcommerce.common.extensibility.context.merge.handlers.InsertItems
priority.14.1=1
xpath.14.1=/beans/bean[@id='blWebTemplateEngine']/property[@name='dialects']/set/ref
```

the XML that is produced would be:

```xml
<bean id="blWebTemplateEngine" class="org.thymeleaf.spring3.SpringTemplateEngine">
    <property name="dialects">
        <set>
            <ref bean="thymeleafSpringStandardDialect" />
            <ref bean="blDialect" />
            <ref bean="myCustomDialect" />
        </set>
    </property>
</bean> 
```

### Which strategy should I use?

When possible, you want to use strategy 1. Whenever the goal is to merge into an existing `ListFactoryBean`, `SetFactoryBean`, or `MapFactoryBean`, this is the right approach.

If you are wanting to contribute an entry to a collection that is not handled by one of those three classes, the next step is to check if an entry in default.properties exists. If it does, you are able to use the second merge strategy.

### Disabling the second strategy

On occassion, you may need to override the Broadleaf merging process.

This can be done by by adding a file named `broadleaf-commerce/skipMergeComponents.txt` in your classpath. For example, in the DemoSite, put this file in the `core/src/main/resources` directory.

The file should contain a list of component names for which you do not want the Broadleaf Commerce merge process to be used. The example below would not perform merging on the `blAddItemWorkflow` and `blUpdateItemWorkflow` components.

```
blAddItemWorkflow
blUpdateItemWorkflow
```

By adding a component to this list, the default Spring merging process will be used.

## Merging Spring MVC Application Contexts

For servlet-level application context files, the second strategy described above is not applicable, as there is no special ContextLoader to process the XML. However, the first strategy is still applicable for any beans that utilize the new FactoryBean approach.
