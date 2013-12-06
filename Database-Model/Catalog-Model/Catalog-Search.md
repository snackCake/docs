# Catalog Search



###Detailed ERD

[![Catalog Search Detail](dataModel/CatalogSearchDetailedERD.png)](_img/dataModel/CatalogSearchDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_SEARCH_FACET     | ^[javadoc:org/broadleafcommerce/core/search/domain/SearchFacet]          | Represents a search facet.   |
|BLC_FIELD            | ^[javadoc:org/broadleafcommerce/core/search/domain/Field]          | Represents a field of a search facet.  |
|BLC_FIELD_SEARCH_TYPES | -        | Designates if the field will be searchable.  |
|BLC_SEARCH_FACET_RANGE | ^[javadoc:org/broadleafcommerce/core/search/domain/SearchFacetRange]        | Designates a range for a search facet.  |
|BLC_SEARCH_FACET_XREF | ^[javadoc:org/broadleafcommerce/core/search/domain/RequiredFacetImpl]       | Cross references required search facets.  |
|BLC_CAT_SEARCH_FACET_XREF   | ^[javadoc:org/broadleafcommerce/core/search/domain/CategorySearchFacet]   | Cross references the search facet with categories. |
|BLC_CAT_SEARCH_FACET_EXCL_XREF| ^[javadoc:org/broadleafcommerce/core/search/domain/CategoryExcludedSearchFacet] | Cross references the search facet with categories to be excluded. |
|BLC_SEARCH_INTERCEPT | ^[javadoc:org/broadleafcommerce/core/search/domain/SearchIntercept]        | Represents the search redirect.  |
|BLC_URL_HANDLER    | ^[javadoc:org/broadleafcommerce/cms/url/domain/URLHandler]        | Represents the URL handler.  |
|BLC_SEARCH_SYNONYM   | ^[javadoc:org/broadleafcommerce/core/search/domain/SearchSynonym]        | Represents search synonym.  |
|BLC_SHIPPING_RATE    | ^[javadoc:org/broadleafcommerce/core/pricing/domain/ShippingRate]         | Represents a shipping rate.  |


###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_CATEGORY         | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Category]          | Represents a category.  |
