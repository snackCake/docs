# Catalog Sku



###Detailed ERD

[![Catalog Sku Detail](dataModel/CatalogSkuDetailedERD.png)](_img/dataModel/CatalogSkuDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_SKU              | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Sku]          | A SKU is a specific item that can be sold including any specific attributes of the item such as color or size.  |
|BLC\_SKU\_ATTRIBUTE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/SkuAttribute]          | A SKU Attribute is a designator on a SKU that differentiates it from other similar SKUs (for example: Blue attribute for hat).  |
|BLC\_SKU\_BUNDLE\_ITEM  | ^[javadoc:org/broadleafcommerce/core/catalog/domain/SkuBundleItem]          | Represents the sku being sold in a bundle.  |
|BLC\_PRODUCT\_BUNDLE   | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductBundle]          | Represents the product being sold in a bundle.  |
|BLC\_SKU\_FULFILLMENT\_FLAT\_RATES| - | Represents the sku fulfillment flat rates.  |
|BLC\_SKU\_AVAILABILITY | ^[javadoc:org/broadleafcommerce/core/inventory/domain/SkuAvailability]      | Represents the availability of the sku.  |
|BLC\_SKU\_FULFILLMENT\_EXCLUDED| -   | Represents if a sku fulfillment is to be excluded.  |
|BLC\_SKU\_OPTION\_VALUE\_XREF   | -   | Represents the cross reference between sku and a product option value. |
|BLC\_PRODUCT\_OPTION\_VALUE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductOptionValue]   | Defines the value of a Product Option.  |
|BLC\_PRODUCT\_OPTION    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductOption]         | Designates a Product Option.  |
|BLC\_SKU\_MEDIA\_MAP    | -          | Maps the sku to a media object.  |
|BLC\_MEDIA            | ^[javadoc:org/broadleafcommerce/core/media/domain/Media]          | Represents a media object.  |
|BLC\_SKU\_FEE\_XREF    | -           | Represents the cross reference between sku and a sku fee. |
|BLC\_SKU\_FEE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/SkuFee]   | Represents a sku fee.  |


###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_PRODUCT          | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Product]          | A product is a general description of an item that can be sold (for example: a hat). Products are not sold or added to a cart.  |
|BLC\_FULFILLMENT\_OPTION | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentOption]        | Represents a fulfillment option.  |
|BLC\_CURRENCY                | ^[javadoc:org/broadleafcommerce/common/currency/domain/BroadleafCurrency]      | Contains currency information, such as code and if it's default  |
