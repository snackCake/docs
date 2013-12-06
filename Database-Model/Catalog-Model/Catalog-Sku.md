# Catalog Sku



###Detailed ERD

[![Catalog Sku Detail](dataModel/CatalogSkuDetailedERD.png)](_img/dataModel/CatalogSkuDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_SKU              | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Sku]          | A SKU is a specific item that can be sold including any specific attributes of the item such as color or size.  |
|BLC_SKU_ATTRIBUTE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/SkuAttribute]          | A SKU Attribute is a designator on a SKU that differentiates it from other similar SKUs (for example: Blue attribute for hat).  |
|BLC_SKU_BUNDLE_ITEM  | ^[javadoc:org/broadleafcommerce/core/catalog/domain/SkuBundleItem]          | Represents the sku being sold in a bundle.  |
|BLC_PRODUCT_BUNDLE   | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductBundle]          | Represents the product being sold in a bundle.  |
|BLC_SKU_FULFILLMENT_FLAT_RATES| - | Represents the sku fulfillment flat rates.  |
|BLC_SKU_AVAILABILITY | ^[javadoc:org/broadleafcommerce/core/inventory/domain/SkuAvailability]      | Represents the availability of the sku.  |
|BLC_SKU_FULFILLMENT_EXCLUDED| -   | Represents if a sku fulfillment is to be excluded.  |
|BLC_SKU_OPTION_VALUE_XREF   | -   | Represents the cross reference between sku and a product option value. |
|BLC_PRODUCT_OPTION_VALUE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductOptionValue]   | Defines the value of a Product Option.  |
|BLC_PRODUCT_OPTION    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductOption]         | Designates a Product Option.  |
|BLC_SKU_MEDIA_MAP    | -          | Maps the sku to a media object.  |
|BLC_MEDIA            | ^[javadoc:org/broadleafcommerce/core/media/domain/Media]          | Represents a media object.  |
|BLC_SKU_FEE_XREF    | -           | Represents the cross reference between sku and a sku fee. |
|BLC_SKU_FEE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/SkuFee]   | Represents a sku fee.  |


###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_PRODUCT          | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Product]          | A product is a general description of an item that can be sold (for example: a hat). Products are not sold or added to a cart.  |
|BLC_FULFILLMENT_OPTION | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentOption]        | Represents a fulfillment option.  |
|BLC_CURRENCY                | ^[javadoc:org/broadleafcommerce/common/currency/domain/BroadleafCurrency]      | Contains currency information, such as code and if it's default  |
