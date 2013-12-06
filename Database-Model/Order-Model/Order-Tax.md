# Order Tax

###Detailed ERD

[![Order Tax](dataModel/OrderTaxDetailedERD.png)](_img/dataModel/OrderTaxDetailedERD.png)

###Tables

| Table               | Related Entity | Description                                         |
|:--------------------|:----------|:----------------------------------------------------|
|BLC_FG_FEE_TAX_XREF  | n/a      | Cross-reference from fulfillment group fees to tax details  |
|BLC_FG_FG_TAX_XREF   | n/a      | Cross-reference from fulfillment group to tax details  |
|BLC_FG_ITEM_TAX_XREF | n/a      | Cross-reference from fulfillment group item to tax details |
|BLC_TAX_DETAIL       | ^[javadoc:org/broadleafcommerce/core/order/domain/TaxDetail]      | Contains tax information  |

###Related Tables

| Table                     | Related Entity        | Description                                         |
|:--------------------------|:--------------|:----------------------------------------------------|
|BLC_FULFILLMENT_GROUP_FEE  | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroupFee]          | Contains fee information for a fulfillment group  |
|BLC_FULFILLMENT_GROUP_ITEM | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroupItem]          | Contains information for items in a fulfillment group  |
|BLC_FULFILLMENT_GROUP      | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroup]          | Holds fulfillment information about an order  |
