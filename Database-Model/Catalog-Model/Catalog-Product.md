# Catalog Product



###Detailed ERD

[![Catalog Product Detail](dataModel/CatalogProductDetailedERD.png)](_img/dataModel/CatalogProductDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_PRODUCT          | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Product]          | A product is a general description of an item that can be sold (for example: a hat). Products are not sold or added to a cart.  |
|BLC_PRODUCT_ATTRIBUTE  | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductAttribute]       | Defines attributes for a product.  |
|BLC_SKU_XREF | -             | Cross reference table that points to the skus for the product.  |
|BLC_PRODUCT_BUNDLE   | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductBundle]          | Represents the product being sold in a bundle.  |
|BLC_SKU_OPTION_VALUE_XREF   | -   | Represents the cross reference between sku and a product option value. |
|BLC_PRODUCT_OPTION_VALUE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductOptionValue]   | Defines the value of a Product Option.  |
|BLC_PRODUCT_OPTION    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductOption]         | Designates a Product Option.  |
|BLC_PRODUCT_CROSS_SALE | ^[javadoc:org/broadleafcommerce/core/catalog/domain/RelatedProduct]        | Represents the products in the category.  |
|BLC_PRODUCT_UP_SALE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/RelatedProduct]         | Represents the products in the category.  |
|BLC_PRODUCT_FEATURED   | ^[javadoc:org/broadleafcommerce/core/catalog/domain/PromotableProduct]      | Represents the products in the category.  |




###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_CATEGORY         | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Category]          | Represents a category.  |
|BLC_SKU              | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Sku]          | A SKU is a specific item that can be sold including any specific attributes of the item such as color or size.  |
