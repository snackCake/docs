# Catalog Sku



###Detailed ERD

[![Catalog Sku Detail](dataModel/CatalogSkuDetailedERD.png)](_img/dataModel/CatalogSkuDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_SKU              | [Sku.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Sku.html)          | A SKU is a specific item that can be sold including any specific attributes of the item such as color or size.  |
|BLC\_SKU\_ATTRIBUTE    | [SkuAttribute.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/SkuAttribute.html)          | A SKU Attribute is a designator on a SKU that differentiates it from other similar SKUs (for example: Blue attribute for hat).  |
|BLC\_SKU\_BUNDLE\_ITEM  | [SkuBundleItem.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/SkuBundleItem.html)          | Represents the sku being sold in a bundle.  |
|BLC\_PRODUCT\_BUNDLE   | [ProductBundle.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/ProductBundle.html)          | Represents the product being sold in a bundle.  |
|BLC\_SKU\_FULFILLMENT\_FLAT\_RATES| - | Represents the sku fulfillment flat rates.  |
|BLC\_SKU\_AVAILABILITY | [SkuAvailability.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/inventory/domain/SkuAvailability.html)      | Represents the availability of the sku.  |
|BLC\_SKU\_FULFILLMENT\_EXCLUDED| -   | Represents if a sku fulfillment is to be excluded.  |
|BLC\_SKU\_OPTION\_VALUE\_XREF   | -   | Represents the cross reference between sku and a product option value. |
|BLC\_PRODUCT\_OPTION\_VALUE    | [ProductOptionValue.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/ProductOptionValue.html)   | Defines the value of a Product Option.  |
|BLC\_PRODUCT\_OPTION    | [ProductOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/ProductOption.html)         | Designates a Product Option.  |
|BLC\_SKU\_MEDIA\_MAP    | -          | Maps the sku to a media object.  |
|BLC\_MEDIA            | [Media.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/media/domain/Media.html)          | Represents a media object.  |
|BLC\_SKU\_FEE\_XREF    | -           | Represents the cross reference between sku and a sku fee. |
|BLC\_SKU\_FEE    | [SkuFee.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/SkuFee.html)   | Represents a sku fee.  |


###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_PRODUCT          | [Product.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Product.html)          | A product is a general description of an item that can be sold (for example: a hat). Products are not sold or added to a cart.  |
|BLC\_FULFILLMENT\_OPTION | [FulfillmentOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/FulfillmentOption.html)        | Represents a fulfillment option.  |
|BLC\_CURRENCY                | [BroadleafCurrency.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/currency/domain/BroadleafCurrency.html)      | Contains currency information, such as code and if it's default  |
