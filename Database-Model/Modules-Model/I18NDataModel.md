###Detailed ERD

[![I18n Details](dataModel/modules/I18nDetailedERD.jpeg)](_img/dataModel/modules/I18nDetailedERD.jpeg)

###Tables

| Table                      | Related Entity | Description                                         |
|:---------------------------|:----------|:----------------------------------------------------|
|BLC\_CATEGORY\_TRXREF     | n/a     | Cross references category to translation  |
|BLC\_CATEGORY\_TR     | [CategoryTranslation.java](http://javadoc.broadleafcommerce.org/modules/I18n/current/org/broadleafcommerce/i18n/domain/catalog/CategoryTranslation.html)      |Holds the translation  |
|BLC\_PRODUCT\_OPTION\_VAL\_TRXREF     | n/a     | Cross references product option value to translation  |
|BLC\_PROUCT\_OPTION\_VAL\_TR     | [ProductOptionValueTranslation.java](http://javadoc.broadleafcommerce.org/modules/I18n/current/org/broadleafcommerce/i18n/domain/catalog/ProductOptionValueTranslation.html)      |Holds the translation  |
|BLC\_FULFILLMENT\_OPTION\_TRXREF     | n/a     | Cross references fulfillment to translation  |
|BLC\_FULFILLMENT\_OPTION\_TR     | [FulfillmentOptionTranslation.java](http://javadoc.broadleafcommerce.org/modules/I18n/current/org/broadleafcommerce/i18n/domain/catalog/FulfillmentOptionTranslation.html)      |Holds the translation  |
|BLC\_PRODUCT\_OPTION\_TRXREF     | n/a     | Cross references product option to translation  |
|BLC\_PRODUCT\_OPTION\_TR     | [ProductOptionTranslation.java](http://javadoc.broadleafcommerce.org/modules/I18n/current/org/broadleafcommerce/i18n/domain/catalog/ProductOptionTranslation.html)      |Holds the translation  |
|BLC\_SKU\_TRANSLATION\_XREF     | n/a     | Cross references sku to translation  |
|BLC\_SKU\_TRANSLATION     | [SkuTranslation.java](http://javadoc.broadleafcommerce.org/modules/I18n/current/org/broadleafcommerce/i18n/domain/catalog/SkuTranslation.html)      |Holds the translation  |
|BLC\_SEARCH\_FACET     | n/a     | Cross references search facet to translation  |
|BLC\_SEARCH\_FACET\_TRXREF     | [SearchFacetTranslation.java](http://javadoc.broadleafcommerce.org/modules/I18n/current/org/broadleafcommerce/i18n/domain/search/SearchFacetTranslation.html)      |Holds the translation  |
###Related Tables

| Table                | Related Entity    | Description                                         |
|:---------------------|:--------------|:----------------------------------------------------|
|BLC\_CATEGORY           | [Category.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Category.html)           | Contains Category  |
|BLC\_SKU          | [Sku.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Sku.html)          |  A SKU is a specific item that can be sold including any specific attributes of the item such as color or size.    |
|BLC\_PRODUCT\_OPTION\_VALUE          | [ProductOptionValue.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/ProductOptionValue.html)          | Represents a product option value  |
|BLC\_PRODUCT\_OPTION          | [ProductOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/ProductOption.html)          | Represents a product option  |
|SEARCH\_FACET | [SearchFacet.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/search/domain/SearchFacet.html)          | Holds search facet  information  |
|BLC\_FULFILLMENT\_OPTION             | [FulfillmentOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/FulfillmentOption.html)          | Represents an order  Fullfillment option  |
