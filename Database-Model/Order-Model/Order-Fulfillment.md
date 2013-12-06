# Order Fulfillment

###Detailed ERD

[![Order Fulfillment](dataModel/OrderFulfillmentDetailedERD.png)](_img/dataModel/OrderFulfillmentDetailedERD.png)

###Tables

| Table                         | Related Entity | Description                                         |
|:------------------------------|:----------|:----------------------------------------------------|
|BLC\_DYN\_DISCRETE\_ORDER\_ITEM    | ^[javadoc:org/broadleafcommerce/core/order/domain/DynamicPriceDiscreteOrderItem]      | Contains discrete order item information that is dynamically priced  |
|BLC\_FULFILLMENT\_GROUP          | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroup]      | Holds fulfillment information about an order  |
|BLC\_FULFILLMENT\_GROUP\_FEE      | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroupFee]      | Contains fee information for a fulfillment group  |
|BLC\_FULFILLMENT\_GROUP\_ITEM     | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroupItem]      | Contains information for items in a fulfillment group  |
|BLC\_FULFILLMENT\_OPTION         | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentOption]      | Holds information about a particular fulfillment implementation  |
|BLC\_FULFILLMENT\_OPTION\_FIXED   | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/FixedPriceFulfillmentOption]      | Contains single-price data for order fulfillment  |
|BLC\_FULFILLMENT\_OPT\_BANDED\_PRC | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/BandedPriceFulfillmentOption]      | Contains fulfillment option data by price band  |
|BLC\_FULFILLMENT\_OPT\_BANDED\_WGT | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/BandedWeightFulfillmentOption]      | Contains fulfillment option data by weight band  |
|BLC\_FULFILLMENT\_PRICE\_BAND     | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/FulfillmentPriceBand]      | Contains pricing bands based on retail price of a fulfillment group |
|BLC\_FULFILLMENT\_WEIGHT\_BAND    | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/FulfillmentWeightBand]      | Contains pricing bands based on weight of a fulfillment group |

###Related Tables

| Table                | Related Entity    | Description                                         |
|:---------------------|:--------------|:----------------------------------------------------|
|BLC\_ADDRESS           | ^[javadoc:org/broadleafcommerce/profile/core/domain/Address]           | Contains address information, e.g. city, state, and postal code  |
|BLC\_ORDER             | ^[javadoc:org/broadleafcommerce/core/order/domain/Order]          | Represents an order in Broadleaf  |
|BLC\_PERSONAL\_MESSAGE  | ^[javadoc:org/broadleafcommerce/core/order/domain/PersonalMessage]          | Contains personal message information (e.g. from, to, message body)  |
|BLC\_PHONE             | ^[javadoc:org/broadleafcommerce/profile/core/domain/Phone]          | Represents a phone number in broadleaf  |
|BLC\_ORDER\_ITEM        | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderItem]          | An abstract representation of an item on an order  |
|BLC\_CURRENCY                | ^[javadoc:org/broadleafcommerce/common/currency/domain/BroadleafCurrency]      | Contains currency information, such as code and if it's default  |
