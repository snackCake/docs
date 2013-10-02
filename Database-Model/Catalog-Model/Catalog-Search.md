# Catalog Search



###Detailed ERD

[![Catalog Search Detail](dataModel/CatalogSearchDetailedERD.png)](_img/dataModel/CatalogSearchDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_SEARCH\_FACET     | [SearchFacet.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/search/domain/SearchFacet.html)          | Represents a search facet.   |
|BLC\_FIELD            | [Field.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/search/domain/Field.html)          | Represents a field of a search facet.  |
|BLC\_FIELD\_SEARCH\_TYPES | -        | Designates if the field will be searchable.  |
|BLC\_SEARCH\_FACET\_RANGE | [SearchFacetRange.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/search/domain/SearchFacetRange.html)        | Designates a range for a search facet.  |
|BLC\_SEARCH\_FACET\_XREF | [RequiredFacetImpl.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/search/domain/RequiredFacetImpl.html)       | Cross references required search facets.  |
|BLC\_CAT\_SEARCH\_FACET\_XREF   | [CategorySearchFacet.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/search/domain/CategorySearchFacet.html)   | Cross references the search facet with categories. |
|BLC\_CAT\_SEARCH\_FACET\_EXCL\_XREF| [CategoryExcludedSearchFacet.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/search/domain/CategoryExcludedSearchFacet.html) | Cross references the search facet with categories to be excluded. |
|BLC\_SEARCH\_INTERCEPT | [SearchIntercept.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/search/domain/SearchIntercept.html)        | Represents the search redirect.  |
|BLC\_URL\_HANDLER    | [URLHandler.java](http://javadoc.broadleafcommerce.org/current/contentmanagement-module/org/broadleafcommerce/cms/url/domain/URLHandler.html)        | Represents the URL handler.  |
|BLC\_SEARCH\_SYNONYM   | [SearchSynonym.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/search/domain/SearchSynonym.html)        | Represents search synonym.  |
|BLC\_SHIPPING\_RATE    | [ShippingRate.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/pricing/domain/ShippingRate.html)         | Represents a shipping rate.  |


###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_CATEGORY         | [Category.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Category.html)          | Represents a category.  |
