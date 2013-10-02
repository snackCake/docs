# Order Tax

###Detailed ERD

[![Order Tax](dataModel/OrderTaxDetailedERD.png)](_img/dataModel/OrderTaxDetailedERD.png)

###Tables

| Table               | Related Entity | Description                                         |
|:--------------------|:----------|:----------------------------------------------------|
|BLC\_FG\_FEE\_TAX\_XREF  | n/a      | Cross-reference from fulfillment group fees to tax details  |
|BLC\_FG\_FG\_TAX\_XREF   | n/a      | Cross-reference from fulfillment group to tax details  |
|BLC\_FG\_ITEM\_TAX\_XREF | n/a      | Cross-reference from fulfillment group item to tax details |
|BLC\_TAX\_DETAIL       | [TaxDetail.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/TaxDetail.html)      | Contains tax information  |

###Related Tables

| Table                     | Related Entity        | Description                                         |
|:--------------------------|:--------------|:----------------------------------------------------|
|BLC\_FULFILLMENT\_GROUP\_FEE  | [FulfillmentGroupFee.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/FulfillmentGroupFee.html)          | Contains fee information for a fulfillment group  |
|BLC\_FULFILLMENT\_GROUP\_ITEM | [FulfillmentGroupItem](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/FulfillmentGroupItem.html)          | Contains information for items in a fulfillment group  |
|BLC\_FULFILLMENT\_GROUP      | [FulfillmentGroup](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/FulfillmentGroup.html)          | Holds fulfillment information about an order  |
|BLC\_CURRENCY                | [BroadleafCurrency.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/currency/domain/BroadleafCurrency.html)      | Contains currency information, such as code and if it's default  |
