# Order Fulfillment

###Detailed ERD

[![Order Fulfillment](dataModel/OrderFulfillmentDetailedERD.png)](_img/dataModel/OrderFulfillmentDetailedERD.png)

###Tables

| Table                         | Related Entity | Description                                         |
|:------------------------------|:----------|:----------------------------------------------------|
|BLC_DYN_DISCRETE_ORDER_ITEM    | ^[javadoc:org/broadleafcommerce/core/order/domain/DynamicPriceDiscreteOrderItem]      | Contains discrete order item information that is dynamically priced  |
|BLC_FULFILLMENT_GROUP          | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroup]      | Holds fulfillment information about an order  |
|BLC_FULFILLMENT_GROUP_FEE      | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroupFee]      | Contains fee information for a fulfillment group  |
|BLC_FULFILLMENT_GROUP_ITEM     | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroupItem]      | Contains information for items in a fulfillment group  |
|BLC_FULFILLMENT_OPTION         | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentOption]      | Holds information about a particular fulfillment implementation  |
|BLC_FULFILLMENT_OPTION_FIXED   | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/FixedPriceFulfillmentOption]      | Contains single-price data for order fulfillment  |
|BLC_FULFILLMENT_OPT_BANDED_PRC | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/BandedPriceFulfillmentOption]      | Contains fulfillment option data by price band  |
|BLC_FULFILLMENT_OPT_BANDED_WGT | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/BandedWeightFulfillmentOption]      | Contains fulfillment option data by weight band  |
|BLC_FULFILLMENT_PRICE_BAND     | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/FulfillmentPriceBand]      | Contains pricing bands based on retail price of a fulfillment group |
|BLC_FULFILLMENT_WEIGHT_BAND    | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/FulfillmentWeightBand]      | Contains pricing bands based on weight of a fulfillment group |

###Related Tables

| Table                | Related Entity    | Description                                         |
|:---------------------|:--------------|:----------------------------------------------------|
|BLC_ADDRESS           | ^[javadoc:org/broadleafcommerce/profile/core/domain/Address]           | Contains address information, e.g. city, state, and postal code  |
|BLC_ORDER             | ^[javadoc:org/broadleafcommerce/core/order/domain/Order]          | Represents an order in Broadleaf  |
|BLC_PERSONAL_MESSAGE  | ^[javadoc:org/broadleafcommerce/core/order/domain/PersonalMessage]          | Contains personal message information (e.g. from, to, message body)  |
|BLC_PHONE             | ^[javadoc:org/broadleafcommerce/profile/core/domain/Phone]          | Represents a phone number in broadleaf  |
|BLC_ORDER_ITEM        | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderItem]          | An abstract representation of an item on an order  |
