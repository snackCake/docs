

###Detailed ERD

[![Catalog Category Detail](dataModel/modules/CatalogMetaDataCatalogCategoryDetailedERD.png)](_img/dataModel/modules/CatalogMetaDataCatalogCategoryDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_CATEGORY         | [Category.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Category.html)          | Represents a category.  |
|BLC\_CATEGORY\_ATTRIBUTE  | [CategoryAttribute.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/CategoryAttribute.html)       | Defines attributes for a category.  |
|BLC\_CATEGORY\_IMAGE   | -          | Represents a URL to an image for the category.  |
|BLC\_CATEGORY\_XREF | -             | Cross reference table that points to the subcategories of each category.  |
|BLC\_PRODUCT\_CROSS\_SALE | [RelatedProduct.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/RelatedProduct.html)        | Represents the products in the category.  |
|BLC\_PRODUCT\_UP\_SALE    | [RelatedProduct.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/RelatedProduct.html)         | Represents the products in the category.  |
|BLC\_PRODUCT\_FEATURED   | [PromotableProduct.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/PromotableProduct.html)      | Represents the products in the category.  |




###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_PRODUCT          | [Product.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Product.html)          | A product is a general description of an item that can be sold (for example: a hat). Products are not sold or added to a cart.  |
|BLC\_MEDIA            | [Media.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/media/domain/Media.html)          | Represents a media object.  |
