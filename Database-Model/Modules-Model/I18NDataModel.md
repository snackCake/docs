###Detailed ERD

[![I18n Details](images/dataModel/modules/I18nDetailedERD.png)](images/dataModel/modules/I18nDetailedERD.png)

###Tables

| Table                      | Related Entity | Description                                         |
|:---------------------------|:----------|:----------------------------------------------------|
|BLC_CATEGORY_TRXREF     | n/a     | Cross references category to translation  |
|BLC_CATEGORY_TR     | [CategoryTranslation.java](http://javadoc.broadleafcommerce.org/modules/I18n/current/org/broadleafcommerce/i18n/domain/catalog/CategoryTranslation.html)      |Holds the translation  |
|BLC_PRODUCT_OPTION_VAL_TRXREF     | n/a     | Cross references product option value to translation  |
|BLC_PROUCT_OPTION_VAL_TR     | [ProductOptionValueTranslation.java](http://javadoc.broadleafcommerce.org/modules/I18n/current/org/broadleafcommerce/i18n/domain/catalog/ProductOptionValueTranslation.html)      |Holds the translation  |
|BLC_FULFILLMENT_OPTION_TRXREF     | n/a     | Cross references fulfillment to translation  |
|BLC_FULFILLMENT_OPTION_TR     | [FulfillmentOptionTranslation.java](http://javadoc.broadleafcommerce.org/modules/I18n/current/org/broadleafcommerce/i18n/domain/catalog/FulfillmentOptionTranslation.html)      |Holds the translation  |
|BLC_PRODUCT_OPTION_TRXREF     | n/a     | Cross references product option to translation  |
|BLC_PRODUCT_OPTION_TR     | [ProductOptionTranslation.java](http://javadoc.broadleafcommerce.org/modules/I18n/current/org/broadleafcommerce/i18n/domain/catalog/ProductOptionTranslation.html)      |Holds the translation  |
|BLC_SKU_TRANSLATION_XREF     | n/a     | Cross references sku to translation  |
|BLC_SKU_TRANSLATION     | [SkuTranslation.java](http://javadoc.broadleafcommerce.org/modules/I18n/current/org/broadleafcommerce/i18n/domain/catalog/SkuTranslation.html)      |Holds the translation  |
|BLC_SEARCH_FACET     | n/a     | Cross references search facet to translation  |
|BLC_SEARCH_FACET_TRXREF     | [SearchFacetTranslation.java](http://javadoc.broadleafcommerce.org/modules/I18n/current/org/broadleafcommerce/i18n/domain/search/SearchFacetTranslation.html)      |Holds the translation  |
###Related Tables

| Table                | Related Entity    | Description                                         |
|:---------------------|:--------------|:----------------------------------------------------|
|BLC_CATEGORY           | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Category]           | Contains Category  |
|BLC_SKU          | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Sku]          |  A SKU is a specific item that can be sold including any specific attributes of the item such as color or size.    |
|BLC_PRODUCT_OPTION_VALUE          | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductOptionValue]          | Represents a product option value  |
|BLC_PRODUCT_OPTION          | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductOption]          | Represents a product option  |
|SEARCH_FACET | ^[javadoc:org/broadleafcommerce/core/search/domain/SearchFacet]          | Holds search facet  information  |
|BLC_FULFILLMENT_OPTION             | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentOption]          | Represents an order  Fullfillment option  |
