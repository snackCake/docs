# Catalog Category



###Detailed ERD

[![Catalog Category Detail](dataModel/CatalogCategoryDetailedERD.png)](_img/dataModel/CatalogCategoryDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_CATEGORY         | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Category]          | Represents a category.  |
|BLC\_CATEGORY\_ATTRIBUTE  | ^[javadoc:org/broadleafcommerce/core/catalog/domain/CategoryAttribute]       | Defines attributes for a category.  |
|BLC\_CATEGORY\_IMAGE   | -          | Represents a URL to an image for the category.  |
|BLC\_CATEGORY\_XREF | -             | Cross reference table that points to the subcategories of each category.  |
|BLC\_PRODUCT\_CROSS\_SALE | ^[javadoc:org/broadleafcommerce/core/catalog/domain/RelatedProduct]        | Represents the products in the category.  |
|BLC\_PRODUCT\_UP\_SALE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/RelatedProduct]         | Represents the products in the category.  |
|BLC\_PRODUCT\_FEATURED   | ^[javadoc:org/broadleafcommerce/core/catalog/domain/PromotableProduct]      | Represents the products in the category.  |
|BLC\_CAT\_SITE\_MAP\_GEN\_CFG   | ^[javadoc:org/broadleafcommerce/core/catalog/domain/CategorySiteMapGeneratorConfiguration]      | CategorySiteMapGenerator is controlled by this configuration.  |



###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_PRODUCT          | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Product]          | A product is a general description of an item that can be sold (for example: a hat). Products are not sold or added to a cart.  |
|BLC\_MEDIA            | ^[javadoc:org/broadleafcommerce/core/media/domain/Media]          | Represents a media object.  |
