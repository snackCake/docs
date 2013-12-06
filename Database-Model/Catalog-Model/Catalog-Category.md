# Catalog Category



###Detailed ERD

[![Catalog Category Detail](dataModel/CatalogCategoryDetailedERD.png)](_img/dataModel/CatalogCategoryDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_CATEGORY         | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Category]          | Represents a category.  |
|BLC_CATEGORY_ATTRIBUTE  | ^[javadoc:org/broadleafcommerce/core/catalog/domain/CategoryAttribute]       | Defines attributes for a category.  |
|BLC_CATEGORY_IMAGE   | -          | Represents a URL to an image for the category.  |
|BLC_CATEGORY_XREF | -             | Cross reference table that points to the subcategories of each category.  |
|BLC_PRODUCT_CROSS_SALE | ^[javadoc:org/broadleafcommerce/core/catalog/domain/RelatedProduct]        | Represents the products in the category.  |
|BLC_PRODUCT_UP_SALE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/RelatedProduct]         | Represents the products in the category.  |
|BLC_PRODUCT_FEATURED   | ^[javadoc:org/broadleafcommerce/core/catalog/domain/PromotableProduct]      | Represents the products in the category.  |




###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_PRODUCT          | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Product]          | A product is a general description of an item that can be sold (for example: a hat). Products are not sold or added to a cart.  |
|BLC_MEDIA            | ^[javadoc:org/broadleafcommerce/core/media/domain/Media]          | Represents a media object.  |
