###Detailed ERD

[![PriceLists Details](dataModel/modules/PriceListsDetailedERD.png)](_img/dataModel/modules/PriceListsDetailedERD.png)

###Tables

| Table                      | Related Entity | Description                                         |
|:---------------------------|:----------|:----------------------------------------------------|
|BLC\_PRICE\_LIST       | [PriceList.java](http://javadoc.broadleafcommerce.org/modules/PriceList/current/domain/PriceList.html)      | Defines a unique pricelist based on the PRICE\_KEY|
|BLC\_PRICE\_DATA     | [PriceData.java](http://javadoc.broadleafcommerce.org/modules/PriceList/current/domain/PriceData.html)      |Holds the sale and retail price that will be dynamically priced  |
|BLC\_PRICE\_ADJUSTMENT | [PriceAdjustment.java](http://javadoc.broadleafcommerce.org/modules/PriceList/current/domain/PriceAdjustment.html)      | Contains sku price information that is dynamically priced  |
|BLC\_ADJ\_PRICE\_XREF           | [n/a]      | Cross references product option value to price adjustment  |
|BLC\_OFFER\_PRICELIST\_XREF     | [n/a]    | Cross references offer to prices list   |
|BLC\_SKU\_BUNDLE\_PRICE\_DATA     | n/a       | Cross reference from sku bundle item to price data  |
|BLC\_SKU\_PRICE\_DATA | [PriceAdjustment.java](http://javadoc.broadleafcommerce.org/modules/PriceList/current/domain/SkuBundleItemPriceData.html)      | Contains sku bundle item price information that is dynamically priced  |
###Related Tables

| Table                | Related Entity    | Description                                         |
|:---------------------|:--------------|:----------------------------------------------------|
|BLC\_SKU\_BUNDLE\_ITEM           | [SkuBundleItem.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/SkuBundleItem.html)           | Contains address information, e.g. city, state, and postal code  |
|BLC\_SKU          | [Sku.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Sku.html)          |  A SKU is a specific item that can be sold including any specific attributes of the item such as color or size.    |
|BLC\_PRODUCT\_OPTION\_VALUE          | [ProductOptionValue.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/ProductOptionValue.html)          | Represents a customer in Broadleaf  |
|BLC\_CUSTOMER| [Customer.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Customer.html)          | Represents a customer in Broadleaf |
|SEARCH\_FACET\_RANGE | [SearchFacetRange.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/search/domain/SearchFacetRange.html)          | Holds search facet range information  |
|BLC\_ORDER             | [Order.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/Order.html)          | Represents an order in Broadleaf  |
