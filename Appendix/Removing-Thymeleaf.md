# Removing Thymeleaf

Although the Broadleaf Commerce DemoSite shows a front-end that was built with the Thymeleaf tempalting engine, it is certainly possible to use a different templating language if you choose.

If you would like to completely remove depdencency on Thymeleaf JARs from the *site* project, follow these steps:

1. Add the pom exclusion in your site pom.xml:

    ```xml
    <exclusions>
        <exclusion>
            <artifactId>thymeleaf-spring3</artifactId>
            <groupId>org.thymeleaf</groupId>
        </exclusion>
        <exclusion>
            <artifactId>thymeleaf</artifactId>
            <groupId>org.thymeleaf</groupId>
        </exclusion>
    </exclusions>
    ```

2. Delete the following bean from `applicationContext-servlet.xml`:

    ```xml
    <bean class="org.broadleafcommerce.common.web.BroadleafThymeleafViewResolver">
    ```

3. Add the following bean overrides to your `applicationContext.xml` file:

    ```xml
    <bean id="blVariableExpressionEvaluator" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blDialect" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blVariableExpressions" class="org.broadleafcommerce.common.util.NullFactoryBean" />

    <bean id="blWebTemplateResolvers" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blWebCustomTemplateResolver" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blWebTemplateResolver" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blWebClasspathTemplateResolver" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blWebCommonClasspathTemplateResolver" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blWebTemplateEngine" class="org.broadleafcommerce.common.util.NullFactoryBean" />

    <bean id="blEmailTemplateResolvers" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blEmailTemplateResolver" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blEmailCustomTemplateResolver" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blEmailClasspathTemplateResolver" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blEmailTemplateEngine" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    
    <bean id="thymeleafSpringStandardDialect" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blMessageResolver" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    
    <bean id="blContentProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blDataDrivenEnumerationProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blResourceBundleProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    
    <bean id="blAddSortLinkProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blCategoriesProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blFormProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blGoogleAnalyticsProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blHeadProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blNamedOrderProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blProductOptionDisplayProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blProductOptionValueProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blProductOptionsProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blPaginationPageLinkProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blPriceTextDisplayProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blRatingsProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blRelatedProductProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blRemoveFacetValuesLinkProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blToggleFacetLinkProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blUrlRewriteProcessor" class="org.broadleafcommerce.common.util.NullFactoryBean" />

    <bean id="blHeadProcessorExtensionManager" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    <bean id="blContentProcessorExtensionManager" class="org.broadleafcommerce.common.util.NullFactoryBean" />
    ```
