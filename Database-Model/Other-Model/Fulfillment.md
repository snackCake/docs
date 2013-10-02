# Fulfillment



###Detailed ERD

[![Fulfillment Detail](dataModel/FulfillmentDetailedERD.png)](_img/dataModel/FulfillmentDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_FULFILLMENT\_OPTION | [FulfillmentOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/FulfillmentOption.html)        | Holds information about a particular fulfillment implementation.  |
|BLC\_FULFILLMENT\_OPT\_BANDED\_PRC | [BandedPriceFulfillmentOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/fulfillment/domain/BandedPriceFulfillmentOption.html)| Links to a list of Fulfillment Price Bands.  |
|BLC\_FULFILLMENT\_OPT\_BANDED\_WGT | [BandedWeightFulfillmentOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/fulfillment/domain/BandedWeightFulfillmentOption.html)| Links to a list of Fulfillment Weight Bands.  |
|BLC\_FULFILLMENT\_OPTION\_FIXED| [FixedPriceFulfillmentOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/fulfillment/domain/FixedPriceFulfillmentOption.html)   | Represents a Fixed Fulfillment Option.  |
|BLC\_FULFILLMENT\_PRICE\_BAND  | [FulfillmentPriceBand.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/fulfillment/domain/FulfillmentPriceBand.html)   | Represents a Fulfillment Price Band.  |
|BLC\_FULFILLMENT\_WEIGHT\_BAND | [FulfillmentWeightBand.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/fulfillment/domain/FulfillmentWeightBand.html)   | Represents a Fulfillment Weight Band.  |
