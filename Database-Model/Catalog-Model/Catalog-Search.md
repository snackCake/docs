# Catalog Search



###Detailed ERD

[![Catalog Search Detail](dataModel/CatalogSearchDetailedERD.png)](_img/dataModel/CatalogSearchDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_SEARCH\_FACET     | ^[javadoc:org/broadleafcommerce/core/search/domain/SearchFacet]          | Represents a search facet.   |
|BLC\_FIELD            | ^[javadoc:org/broadleafcommerce/core/search/domain/Field]          | Represents a field of a search facet.  |
|BLC\_FIELD\_SEARCH\_TYPES | -        | Designates if the field will be searchable.  |
|BLC\_SEARCH\_FACET\_RANGE | ^[javadoc:org/broadleafcommerce/core/search/domain/SearchFacetRange]        | Designates a range for a search facet.  |
|BLC\_SEARCH\_FACET\_XREF | ^[javadoc:org/broadleafcommerce/core/search/domain/RequiredFacetImpl]       | Cross references required search facets.  |
|BLC\_CAT\_SEARCH\_FACET\_XREF   | ^[javadoc:org/broadleafcommerce/core/search/domain/CategorySearchFacet]   | Cross references the search facet with categories. |
|BLC\_CAT\_SEARCH\_FACET\_EXCL\_XREF| ^[javadoc:org/broadleafcommerce/core/search/domain/CategoryExcludedSearchFacet] | Cross references the search facet with categories to be excluded. |
|BLC\_SEARCH\_INTERCEPT | ^[javadoc:org/broadleafcommerce/core/search/domain/SearchIntercept]        | Represents the search redirect.  |
|BLC\_URL\_HANDLER    | ^[javadoc:org/broadleafcommerce/cms/url/domain/URLHandler]        | Represents the URL handler.  |
|BLC\_SEARCH\_SYNONYM   | ^[javadoc:org/broadleafcommerce/core/search/domain/SearchSynonym]        | Represents search synonym.  |
|BLC\_SHIPPING\_RATE    | ^[javadoc:org/broadleafcommerce/core/pricing/domain/ShippingRate]         | Represents a shipping rate.  |


###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_CATEGORY         | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Category]          | Represents a category.  |
