# Fulfillment Groups

Broadleaf Commerce provides a concept called Fulfillment Groups, out of the box. An order (or cart) in Broadleaf can have 1 or more fulfillment groups (see the [[Fulfillment]] data model).  A fulfillment group is a grouping of products, within an order, to be fulfilled in a specific, arbitrary way.  Each fulfillment has, for example, a shipping address, contact information, the the products and their associated quantities that belong to the fulfillment group, shipping option (e.g. standard, 2 day, overnight), taxes, and shipping/handling fees.

In many cases, Broadleaf users will require only one fulfillment group.  For example, you may allow your customers to buy as many items as they wish, but only allow them to ship to a single address.  In other cases, you may allow your customers to split an order up into multiple fulfillments.  For example, a customer may wish to buy something for herself, as well as something for a friend.  In this case, the items will be split into 2 different fulfillment groups at the request of the customer.

However, there are other reasons to have multiple fulfillment groups that may have nothing to do with a customer's desire to ship to multiple addresses.  In these cases there is typically a business need to programatically construct the appropriate fulfillment groups based on a specific set of business requirements.  Let's assume, for example, that you have a customer that has added multiple products to their cart and they wish to check out.  They want all of their products shipped to a single address.  Let's also assume that you have customized Broadleaf to associate each product with a merchant or vendor.  Each product is also identified as being a drop shipped product or one shipped from a warehouse.  If it's shipped from a warehouse, then the appropriate warehouse must be selected based on inventory and the address it is being shipped to.  Broadleaf provides a programmatic hook to allow you to split products into fulfillment groups based on any arbitrary business logic.  So, if the customer requests a product that needs to be shipped from a warehouse, and 2 products that are from a vendor that need to be drop shipped, then custom logic can determine that how the fulfillment groups are constructed without the customer needing to have any knowledge of this.

One final example is non physical products such as a digital product.  Since a customer may sell physical and digital products, the fulfillment of these are very different.  If a customer were to purchase physcal and digital products in a single order, splitting them into fulfillment groups is very beneficial for managing the meta data for each group and discretely integrating with various back-end fulfillment systems as required.


## Shipping and Taxes

Taxes, special fees, and shipping (fulfillment fees) are calculated for each fulfillment group.  The reason is that, if in a singe order order you are shipping to multiple states or countries, the shipping and / or taxe calculations might be different for each destination.  If you are fulfilling digital products, for example, there may be no shipping or taxes required.

## Fulfillment Group Item Strategy

Of course you can allow your customers to ship to multiple addresses, effectively allowing them to create their own fulfillment groups.  However, as described above, you often want to programmatically construct fulfillment groups (in addition to, or instead of allowing customers to do it).

Broadleaf, by default, constructs fulfillment groups during cart operations (e.g. add to cart) using the interface ```org.broadleafcommerce.core.order.strategy.FulfillmentGroupItemStrategy```.  There are two out of the box implementations:

- ```org.broadleafcommerce.core.order.strategy.NullFulfillmentGroupItemStrategyImpl``` which does not create fulfillemnt groups at all
- ```org.broadleafcommerce.core.order.strategy.FulfillmentGroupItemStrategyImpl``` which attempts to create fulfillment groups based on the fulfillment type

Fulfillment type is a property of the Sku and is an extensible Broadleaf Enumeration that, by default, has the following values:

- DIGITAL (to indicate digital goods)
- PHYSICAL\_PICKUP (to indicate, typically, goods to be picked up at a store)
- PHYSICAL\_SHIP (to indicate, typically, goods shipped from a warehouse)
- PHYSICAL\_PICKUP\_OR\_SHIP (goods that can potentially be picked up at a store or shipped, depending on the inventory system)
- GIFT\_CARD (indicates items that are fulfilled as a gift card)

You can add additional values if required (e.g. PHYSICAL\_DROP\_SHIP to differentiate warehouse from drop ship).

In order to implement a custom Fulfillment Group Item Strategy, you can simply extend one of the implementations above and override the appropriate method, or you can directly implement the ```org.broadleafcommerce.core.order.strategy.FulfillmentGroupItemStrategy``` interface.  If you override or extend the bean, simply override the Spring Bean definition in core/src/main/resources/applicationContext.xml as follows:

```xml
<bean id="blFulfillmentGroupItemStrategy" class="com.mycompany.mypackage.MyFulfillmentGroupItemStrategy"/>
```

One note about this... You may prefer to programmatically construct fulfillment groups during the checkout flow rather than during a cart operation.  A key reason may be that you often don't know the customer's shipping address until some point in the checkout process.  In order to do this, you will want to create a [[custom service method | Extending-Services]] and invoke this directly from a Controller, the Order Service, or from the [[checkout workflow | Workflows-and-Activities]].


## Fulfillment

Finally, when the order is submitted, it is usually a requirement to notify a fulfillment or warehouse system, ERP, or accounting system.  In some cases these systems know how to split order items into appropriate groups for fulfillment.  If this is the case, then your fulfillment group strategy is generally less important.  However, when it is the responsibility of Broadleaf to notify different fulfillment systems depending on the type of fulfillment, then it is quite helpful to have fulfillment groups constructed accordingly so that you can integrate with the appropriate fulfillment system(s). This is typically a custom integration.  After the order is submitted (completed), there are several options for sending the fulfillment groups for fulfillemnt:

- Create a background job that searches for submitted orders and then iterates over the fulfillment groups, sending each one to the OMS or fulfillment system based on the fulfillment type, vendor, or some other criteria
- Create a Checkout Workflow Activity that integrates with various fulfillment systems at the completion of checkout and sends each fulfillment group to the OMS or fulfillment system based on the fulfillment type, vendor, or some other criteria

The first option is preferred as it is a background task and therefore will have minimal impact on performance.  In addition, if there is an error or some issue, then a retry can occur, again, without affecting performance.

There are any number of possibilities with respect to this kind of integration.  Some customers integrate with complex ERP systems such as SAP.  Others send the order or fulfillment group details in the form a PDF via email to a warehouse or drop ship vendor.  Yet others integrate with custom systems using protocols such as JMS, AMQP, REST, SOAP, EDI, and RPC, and data formats such as XML, JSON, CSV, etc.


