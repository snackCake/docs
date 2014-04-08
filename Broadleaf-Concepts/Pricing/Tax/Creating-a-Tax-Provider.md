# Creating a Tax Provider

## Anatomy of a Tax Provider

Tax providers are utilized by Broadleaf Commerce to interface with an external provider (or some custom algorithm) for tax calculation. The cost of tax can be based on a number of factors including region and dollar total. If you encounter a situation where there is no existing Broadleaf Commerce tax module for your desired tax calculation method, you may find it necessary to develop your own custom tax provider. Let's review the `TaxProvider` interface from the framework:

```java
public Order calculateTaxForOrder(Order order, ModuleConfiguration config) throws TaxException;

public Order commitTaxForOrder(Order order, ModuleConfiguration config) throws TaxException;

public void cancelTax(Order order, ModuleConfiguration config) throws TaxException;
```

* `calculateTaxForOrder` - invoked during the `blPricingWorkflow` to associate `TaxDetail` objects to the `FulfillmentGroup`s and `FulfillmentGroupItem`s within the given order
* `commitTaxForOrder` - invoked during the `blCheckoutWorkflow` in the `CommitTaxActivity` to communicate with an external tax provider that might need to manage tax documents
* `cancelTax` - if an order has already gone through the `CommitTaxActivity` but did not complete the entire `blCheckoutWorkflow` and taxes that have might have been committed to the external provider need to be rolled back

> Some providers (or your custom implementation) may not use the `commitTaxForOrder` or `cancelTax` methods. At a minimum, your `TaxProvider` should create `TaxDetail` objects for the `FulfillmentGroup`s in an order via the `calculateTaxForOrder` method.

### Selecting a provider

Multiple `TaxProviders` can exist in the framework at once. In order to decide which provider to use, `blTaxService` looks up `ModuleConfiguration`s (a database entity) to get a list of the configured modules in the system. The logic works like this:

* Look up all the Module Configurations that have a type of `ModuleConfigurationType.TAX\_CALCULATION`, sorted by `priority`
* Loop through each configuration
* If a configuration is set to the default, use that configuration
* If there is not a default configuration, use the first one in resulting list (the one with the highest priority)
* Take the determined configuration and try to invoke the delegate method by checking `canRespond` for each provider
* If no provider could respond to the determined configuration and there is a requirement to calculate tax (via the `mustCalculate` property on `blTaxService`) then throw a TaxException

This pattern means that each provider has a unique Module Configuration instance to go along with it.


### Hooking up your provider

Add your provider to the `blTaxProviders` bean:

```xml
<bean id="myTaxProviders" class="org.springframework.beans.factory.config.ListFactoryBean">
    <property name="sourceList">
        <list>
            <bean class="com.mycompany.core.tax.provider.MyCustomTaxProvider" />
        </list>
    </property>
</bean>
<bean class="org.broadleafcommerce.common.extensibility.context.merge.LateStageMergeBeanPostProcessor">
    <property name="collectionRef" value="myTaxProviders"/>
    <property name="targetRef" value="blTaxProviders"/>
</bean>
```

You will also need a database table subclass of `AbstractModuleConiguration`. See the docs for [[Extending Entities]] for more information on how to do this. 

> If you want to bypass the database step for `ModuleConfiguration` you can simply override the `blTaxService` bean with a subclass of `TaxServiceImpl` and implement those methods yourself. Rather than a custom `TaxProvider` you would then have a custom `TaxService`

###Calculating tax

When implementing a `TaxProvider`, the key to communicating the tax cost to Broadleaf Commerce is the Order instance passed into the calculateTaxForOrder method. While the specific informational needs for each tax calculation method will differ, you should have access here to the building blocks necessary to calculate any form of tax. Here are some hints:

* Call `order.getFulfillmentGroups()` to get all the fulfillment groups for the order. Multiple fulfillment groups mean that parts of the order are shipping to different locations.
* Call `fulfillmentGroup.getAddress()` to get the shipping address.
- Call `fulfillmentGroup.getItems()` to get all the `FulfillmentGroupItem`s in a `FulfillmentGroup`
- Call `fulfillmentGroupItem.getOrderItem()` to get an OrderItem
- Call `orderItem.getTaxablePrice()` to get the taxable amount for the item

You'll want to iterate through all the `FulfillmentGroupItem` and `FulfillmentGroupFee` objects in every FulfillmentGroup of the Order, creating and setting `TaxDetail` objects when appropriate. You may also have additional taxes that reside on a `FulfillmentGroup`, such as a shipping tax.

### Responding with tax

Broadleaf Commerce expects that the Order instance returned from the calculateTaxForOrder method of your tax module has the pertinent tax cost totals assigned. Your tax module is on the hook for setting the list of `TaxDetail` objects  on:

- Every `FulfillmentGroup`
- Every `FulfillmentGroupItem`
- Every `FulfillmentGroupFee`

The totalTax fields will be set in the pricing workflow during the `TotalActivity`.
