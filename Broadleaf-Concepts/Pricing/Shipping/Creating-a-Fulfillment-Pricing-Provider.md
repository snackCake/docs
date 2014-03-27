# Creating a Fulfillment Pricing Provider

## Anatomy of a Fulfillment Pricing Provider

These pricing providers have a pretty simple service definition:

```java
public interface FulfillmentPricingProvider {

    public FulfillmentGroup calculateCostForFulfillmentGroup(FulfillmentGroup fulfillmentGroup) throws FulfillmentPriceException;

    public boolean canCalculateCostForFulfillmentGroup(FulfillmentGroup fulfillmentGroup, FulfillmentOption option);

    public FulfillmentEstimationResponse estimateCostForFulfillmentGroup(FulfillmentGroup fulfillmentGroup, Set<FulfillmentOption> options) throws FulfillmentPriceException;
    
}
```

One of the most important methods here is the `canCalculateCostForFulfillmentGroup`. The most common way to determine whether or not to return true would be to check the type of the Fulfillment Option that is passed in. Each external fulfillment integration should have its own implementation of one of a Fulfillment Option. Therefore, if you were writing a module that interfaced with something like Fedex, you might implement this method in this way:

```java
public boolean canCalculateCostForFulfillmentGroup(FulfillmentGroup fulfillmentGroup, FulfillmentOption option) {
    return option instanceof FedexFulfillmentOption;
}
```

The actual calculation method is invoked when Broadleaf is ready to "lock in" the prices for the Fulfillment Group, which basically means that this is in the context of the pricing workflow. This means that last thing that your pricing provider should do is set the `fulfillmentPrice` property on the given Fulfillment Group. For instance:

```java
public FulfillmentGroup calculateCostForFulfillmentGroup(FulfillmentGroup fulfillmentGroup) {
    Money fulfillmentCharges = talkToExternalProviderForPrice(fulfillmentGroup);

    // optionally set the retailPrice and salePrice of the fulfillment group
    // fulfillmentGroup.setRetailFulfillmentPrice(fullPriceOfFulfillmentGroup);
    // fulfillmentGroup.setSaleFulfillmentPrice(discountedPriceOfFulfillmentGroup);

    // REQUIRED
    fulfillmentGroup.setFulfillmentPrice(fulfillmentCharges);
}
```

Estimating is designed to return some generic DTO object representing a map of a given Fulfillment Option to some price. This can be used to present to the user if you allow them to select shipping

> In most cases the only way to properly estimate the cost for a Fulfillment Group is to have the user input a shipping address for the Fulfillment Group. This will need to be taken into account when developing your checkout UI

Other tips for calculating/estimating costs for Fulfillment Groups:

- Call fulfillmentGroup.getAddress() to get the shipping address.
- Call fulfillmentGroup.getMethod() to get the general shipping method identifier.
- Call fulfillmentGroup.getFulfillmentGroupItems() to get the list of all items in this fulfillment group.
- Once you have FulfillmentGroupItems, you can call getOrderItem. If the order item is an instance of DiscreteOrderItem, you can call getProduct, which gives you access to the ProductWeight and ProductDimension (if the order item is an instance of BundleOrderItem, then you'll need to iterate through the bundle to get at the DiscreteOrderItems it contains).
