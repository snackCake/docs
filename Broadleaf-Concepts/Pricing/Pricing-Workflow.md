# Pricing Workflow

## Configuration

Let's take a look at the default configuration in Broadleaf Commerce for pricing:

```xml
<bean id="blPricingWorkflow" class="org.broadleafcommerce.core.workflow.SequenceProcessor">
    <property name="processContextFactory">
        <bean class="org.broadleafcommerce.core.pricing.service.workflow.PricingProcessContextFactory"/>
    </property>
    <property name="activities">
        <list>
            <ref bean="blOfferActivity" />                
            <ref bean="blConsolidateFulfillmentFeesActivity" />
            <ref bean="blFulfillmentItemPricingActivity" />                                       
            <ref bean="blFulfillmentGroupMerchandiseTotalActivity" />
            <ref bean="blFulfillmentGroupPricingActivity" />
            <ref bean="blShippingOfferActivity" />
            <ref bean="blTaxActivity" />
            <ref bean="blTotalActivity"/>
            <ref bean="blAdjustOrderPaymentsActivity"/>
        </list>
    </property>
    <property name="defaultErrorHandler">
        <bean class="org.broadleafcommerce.core.workflow.DefaultErrorHandler">
            <property name="unloggedExceptionClasses">
                <list>
                    <value>org.hibernate.exception.LockAcquisitionException</value>
                </list>
            </property>
        </bean>
    </property>
</bean>
```

Pricing workflows are similar to workflows in general in that they require a ProcessContextFactory, a list of activities and an error handler to be configured. Pricing workflows will use an instance of PricingProcessContextFactory. By default, Broadleaf Commerce supplies activities for calculating offers, fulfillment group totals, shipping costs and offers, taxes and grand totals. In addition, Broadleaf Commerce utilizes the DefaultErrorHandler implementation, which simply logs any exceptions to the console and bubbles the exceptions.

## Customization

Most pricing customization will occur in the following three areas:

- Shipping calculation
- Tax calculation
- Introducting custom pricing activities

### Shipping

Please refer to the [[Shipping]] section for information on customizing the shipping configuration for Broadleaf Commerce.

### Tax

Please refer to the [[Tax]] section for information on customizing the tax configuration for Broadleaf Commerce.

### Custom Activities

Please refer to the [[Custom Pricing Activities]] section for information on creating new pricing activities.

### Explicit Execution of Pricing

The pricing workflow is called automatically from several locations in the Broadleaf Commerce codebase. However, you may find from time to time that you need to call pricing explicitly from your own custom code. This is easily accomplished by injecting the PricingService into your custom class by referencing the key id `blPricingService`.

