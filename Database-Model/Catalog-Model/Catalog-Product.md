# Catalog Product



###Detailed ERD

[![Catalog Product Detail](dataModel/CatalogProductDetailedERD.png)](_img/dataModel/CatalogProductDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_PRODUCT          | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Product]          | A product is a general description of an item that can be sold (for example: a hat). Products are not sold or added to a cart.  |
|BLC\_PRODUCT\_ATTRIBUTE  | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductAttribute]       | Defines attributes for a product.  |
|BLC\_SKU\_XREF | -             | Cross reference table that points to the skus for the product.  |
|BLC\_PRODUCT\_BUNDLE   | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductBundle]          | Represents the product being sold in a bundle.  |
|BLC\_SKU\_OPTION\_VALUE\_XREF   | -   | Represents the cross reference between sku and a product option value. |
|BLC\_PRODUCT\_OPTION\_VALUE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductOptionValue]   | Defines the value of a Product Option.  |
|BLC\_PRODUCT\_OPTION    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductOption]         | Designates a Product Option.  |
|BLC\_PRODUCT\_CROSS\_SALE | ^[javadoc:org/broadleafcommerce/core/catalog/domain/RelatedProduct]        | Represents the products in the category.  |
|BLC\_PRODUCT\_UP\_SALE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/RelatedProduct]         | Represents the products in the category.  |
|BLC\_PRODUCT\_FEATURED   | ^[javadoc:org/broadleafcommerce/core/catalog/domain/PromotableProduct]      | Represents the products in the category.  |




###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_CATEGORY         | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Category]          | Represents a category.  |
|BLC\_SKU              | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Sku]          | A SKU is a specific item that can be sold including any specific attributes of the item such as color or size.  |
