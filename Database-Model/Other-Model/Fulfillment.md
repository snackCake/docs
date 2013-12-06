# Fulfillment



###Detailed ERD

[![Fulfillment Detail](dataModel/FulfillmentDetailedERD.png)](_img/dataModel/FulfillmentDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_FULFILLMENT\_OPTION | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentOption]        | Holds information about a particular fulfillment implementation.  |
|BLC\_FULFILLMENT\_OPT\_BANDED\_PRC | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/BandedPriceFulfillmentOption]| Links to a list of Fulfillment Price Bands.  |
|BLC\_FULFILLMENT\_OPT\_BANDED\_WGT | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/BandedWeightFulfillmentOption]| Links to a list of Fulfillment Weight Bands.  |
|BLC\_FULFILLMENT\_OPTION\_FIXED| ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/FixedPriceFulfillmentOption]   | Represents a Fixed Fulfillment Option.  |
|BLC\_FULFILLMENT\_PRICE\_BAND  | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/FulfillmentPriceBand]   | Represents a Fulfillment Price Band.  |
|BLC\_FULFILLMENT\_WEIGHT\_BAND | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/FulfillmentWeightBand]   | Represents a Fulfillment Weight Band.  |
