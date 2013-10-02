# Order Fulfillment

###Detailed ERD

[![Order Fulfillment](dataModel/OrderFulfillmentDetailedERD.png)](_img/dataModel/OrderFulfillmentDetailedERD.png)

###Tables

| Table                         | Related Entity | Description                                         |
|:------------------------------|:----------|:----------------------------------------------------|
|BLC\_DYN\_DISCRETE\_ORDER\_ITEM    | [DynamicPriceDiscreteOrderItem.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/DynamicPriceDiscreteOrderItem.html)      | Contains discrete order item information that is dynamically priced  |
|BLC\_FULFILLMENT\_GROUP          | [FulfillmentGroup](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/FulfillmentGroup.html)      | Holds fulfillment information about an order  |
|BLC\_FULFILLMENT\_GROUP\_FEE      | [FulfillmentGroupFee.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/FulfillmentGroupFee.html)      | Contains fee information for a fulfillment group  |
|BLC\_FULFILLMENT\_GROUP\_ITEM     | [FulfillmentGroupItem](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/FulfillmentGroupItem.html)      | Contains information for items in a fulfillment group  |
|BLC\_FULFILLMENT\_OPTION         | [FulfillmentOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/FulfillmentOption.html)      | Holds information about a particular fulfillment implementation  |
|BLC\_FULFILLMENT\_OPTION\_FIXED   | [FixedPriceFulfillmentOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/fulfillment/domain/FixedPriceFulfillmentOption.html)      | Contains single-price data for order fulfillment  |
|BLC\_FULFILLMENT\_OPT\_BANDED\_PRC | [BandedPriceFulfillmentOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/fulfillment/domain/BandedPriceFulfillmentOption.html)      | Contains fulfillment option data by price band  |
|BLC\_FULFILLMENT\_OPT\_BANDED\_WGT | [BandedWeightFulfillmentOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/fulfillment/domain/BandedWeightFulfillmentOption.html)      | Contains fulfillment option data by weight band  |
|BLC\_FULFILLMENT\_PRICE\_BAND     | [FulfillmentPriceBand.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/fulfillment/domain/FulfillmentPriceBand.html)      | Contains pricing bands based on retail price of a fulfillment group |
|BLC\_FULFILLMENT\_WEIGHT\_BAND    | [FulfillmentWeightBand.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/fulfillment/domain/FulfillmentWeightBand.html)      | Contains pricing bands based on weight of a fulfillment group |

###Related Tables

| Table                | Related Entity    | Description                                         |
|:---------------------|:--------------|:----------------------------------------------------|
|BLC\_ADDRESS           | [Address.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Address.html)           | Contains address information, e.g. city, state, and postal code  |
|BLC\_ORDER             | [Order.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/Order.html)          | Represents an order in Broadleaf  |
|BLC\_PERSONAL\_MESSAGE  | [PersonalMessage.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/PersonalMessage.html)          | Contains personal message information (e.g. from, to, message body)  |
|BLC\_PHONE             | [Phone.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Phone.html)          | Represents a phone number in broadleaf  |
|BLC\_ORDER\_ITEM        | [OrderItem.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/OrderItem.html)          | An abstract representation of an item on an order  |
|BLC\_CURRENCY                | [BroadleafCurrency.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/currency/domain/BroadleafCurrency.html)      | Contains currency information, such as code and if it's default  |
