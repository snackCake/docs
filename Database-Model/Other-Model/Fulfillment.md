# Fulfillment



###Detailed ERD

[![Fulfillment Detail](dataModel/FulfillmentDetailedERD.png)](_img/dataModel/FulfillmentDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_FULFILLMENT_OPTION | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentOption]        | Holds information about a particular fulfillment implementation.  |
|BLC_FULFILLMENT_OPT_BANDED_PRC | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/BandedPriceFulfillmentOption]| Links to a list of Fulfillment Price Bands.  |
|BLC_FULFILLMENT_OPT_BANDED_WGT | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/BandedWeightFulfillmentOption]| Links to a list of Fulfillment Weight Bands.  |
|BLC_FULFILLMENT_OPTION_FIXED| ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/FixedPriceFulfillmentOption]   | Represents a Fixed Fulfillment Option.  |
|BLC_FULFILLMENT_PRICE_BAND  | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/FulfillmentPriceBand]   | Represents a Fulfillment Price Band.  |
|BLC_FULFILLMENT_WEIGHT_BAND | ^[javadoc:org/broadleafcommerce/core/order/fulfillment/domain/FulfillmentWeightBand]   | Represents a Fulfillment Weight Band.  |
