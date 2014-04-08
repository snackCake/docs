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

###Calculating Taxes

When implementing a `TaxProvider`, the key to communicating the tax cost to Broadleaf Commerce is the Order instance passed into the calculateTaxForOrder method. While the specific informational needs for each tax calculation method will differ, you should have access here to the building blocks necessary to calculate any form of tax. Here are some hints:

* Call `order.getFulfillmentGroups()` to get all the fulfillment groups for the order. Multiple fulfillment groups mean that parts of the order are shipping to different locations.
* Call `fulfillmentGroup.getAddress()` to get the shipping address.
- Call `fulfillmentGroup.getItems()` to get all the `FulfillmentGroupItem`s in a `FulfillmentGroup`
- Call `fulfillmentGroupItem.getOrderItem()` to get an OrderItem
- Call `orderItem.getTaxablePrice()` to get the taxable amount for the item

You'll want to iterate through all the `FulfillmentGroupItem` and `FulfillmentGroupFee` objects in every FulfillmentGroup of the Order, creating and setting `TaxDetail` objects when appropriate. You may also have additional taxes that reside on a `FulfillmentGroup`, such as a shipping tax.

### Responding with Tax

Broadleaf Commerce expects that the Order instance returned from the calculateTaxForOrder method of your tax module has the pertinent tax cost totals assigned. Your tax module is on the hook for setting TaxDetail objects (if applicable) on:

- Every `FulfillmentGroup` (Such as a shipping tax)
- Every `FulfillmentGroupItem`
- Every `FulfillmentGroupFee`

The totalTax fields will be set in the pricing workflow during the `TotalActivity`.
