# Order Tax

###Detailed ERD

[![Order Tax](dataModel/OrderTaxDetailedERD.png)](_img/dataModel/OrderTaxDetailedERD.png)

###Tables

| Table               | Related Entity | Description                                         |
|:--------------------|:----------|:----------------------------------------------------|
|BLC\_FG\_FEE\_TAX\_XREF  | n/a      | Cross-reference from fulfillment group fees to tax details  |
|BLC\_FG\_FG\_TAX\_XREF   | n/a      | Cross-reference from fulfillment group to tax details  |
|BLC\_FG\_ITEM\_TAX\_XREF | n/a      | Cross-reference from fulfillment group item to tax details |
|BLC\_TAX\_DETAIL       | ^[javadoc:org/broadleafcommerce/core/order/domain/TaxDetail]      | Contains tax information  |

###Related Tables

| Table                     | Related Entity        | Description                                         |
|:--------------------------|:--------------|:----------------------------------------------------|
|BLC\_FULFILLMENT\_GROUP\_FEE  | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroupFee]          | Contains fee information for a fulfillment group  |
|BLC\_FULFILLMENT\_GROUP\_ITEM | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroupItem]          | Contains information for items in a fulfillment group  |
|BLC\_FULFILLMENT\_GROUP      | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroup]          | Holds fulfillment information about an order  |
|BLC\_CURRENCY                | ^[javadoc:org/broadleafcommerce/common/currency/domain/BroadleafCurrency]      | Contains currency information, such as code and if it's default  |
