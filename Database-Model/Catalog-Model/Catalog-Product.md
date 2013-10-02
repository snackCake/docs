# Catalog Product



###Detailed ERD

[![Catalog Product Detail](dataModel/CatalogProductDetailedERD.png)](_img/dataModel/CatalogProductDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_PRODUCT          | [Product.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Product.html)          | A product is a general description of an item that can be sold (for example: a hat). Products are not sold or added to a cart.  |
|BLC\_PRODUCT\_ATTRIBUTE  | [ProductAttribute.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/ProductAttribute.html)       | Defines attributes for a product.  |
|BLC\_SKU\_XREF | -             | Cross reference table that points to the skus for the product.  |
|BLC\_PRODUCT\_BUNDLE   | [ProductBundle.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/ProductBundle.html)          | Represents the product being sold in a bundle.  |
|BLC\_SKU\_OPTION\_VALUE\_XREF   | -   | Represents the cross reference between sku and a product option value. |
|BLC\_PRODUCT\_OPTION\_VALUE    | [ProductOptionValue.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/ProductOptionValue.html)   | Defines the value of a Product Option.  |
|BLC\_PRODUCT\_OPTION    | [ProductOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/ProductOption.html)         | Designates a Product Option.  |
|BLC\_PRODUCT\_CROSS\_SALE | [RelatedProduct.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/RelatedProduct.html)        | Represents the products in the category.  |
|BLC\_PRODUCT\_UP\_SALE    | [RelatedProduct.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/RelatedProduct.html)         | Represents the products in the category.  |
|BLC\_PRODUCT\_FEATURED   | [PromotableProduct.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/PromotableProduct.html)      | Represents the products in the category.  |




###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_CATEGORY         | [Category.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Category.html)          | Represents a category.  |
|BLC\_SKU              | [Sku.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Sku.html)          | A SKU is a specific item that can be sold including any specific attributes of the item such as color or size.  |
