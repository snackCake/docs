# Admin Permissions

As of v3.1, there was a significant change made to the way permissions work in the Admin. Previously, in order to give security permissions for Users or Roles to view sections of the Admin, multiple permissions needed to be added to said User or Role, and it wasn't exactly clear which permissions were required.  To address this, an extra layer has been added for permissions, which acts as a grouping of the old permissions to match the sections. These groupings of permissions makes up what we refer to as "Friendly Permissions."

Additionally, we have reduced the permission types from CRUD and "All" to be "Read" and "All", or the friendly versions "View" and "Manage."

Here, we have organized how the new friendly permissions are hierarchized.

# Legend

##Section
* Friendly Permission & Type (View or Manage)
	* Permission & Type (Read or All)
		* Entity Permission

# Friendly Permissions

##Category
* [View | Manage] Friendly Category
	* Category [Read | All]
		* Category
		* CategoryAttribute
		* CategoryProductXrefImpl
		* CategoryXrefImpl
		* FeaturedProductImpl
		* CrossSaleProductImpl
		* UpSaleProductImpl
	* Product [Read | All]
		* Product
		* ProductAttribute
		* UpSaleProductImpl
		* SkuBundleItemImpl
	* Search Facet [Read | All]
		* SearchFacet
		* Field
		* CategorySearchFacet
		* SearchFacetRange
		* CategoryExcludedSearchFacet

##Product
* [View | Manage] Friendly Product
	* Product [Read | All]
		* Product
		* ProductAttribute
		* UpSaleProductImpl
		* SkuBundleItemImpl
	* Product Option [Read | All]
		* ProductOption
		* ProductOptionValue
		* ProductOptionXref
	* Sku [Read | All]
		* Sku
	* Currency [Read | All]
		* BroadleafCurrency

##Product Option
* [View | Manage] Product Option
	* Product Option [Read | All]
		* ProductOption
		* ProductOptionValue
		* ProductOptionXref
	* Search Facet [Read | All]
		* SearchFacet
		* Field
		* CategorySearchFacet
		* SearchFacetRange
		* CategoryExcludedSearchFacet

##Offer
* [View | Manage] Offer
	* Offer [Read | All]
		* Offer
		* OfferItemCriteria
		* OfferCode

##Page
* [View | Manage] Page
	* Page [Read | All]
		* Page
		* PageTemplate
		* PageItemCriteria
		* Locale

##Asset
* [View | Manage] Asset
	* Asset [Read | All]
		* StaticAsset
		* StaticAssetFolder

##Structured Content
* [View | Manage] Structured Content
	* Structured Content [Read | All]
		* StructuredContent
		* StructuredContentType
		* StructuredContentItemCriteria
		* StructuredContentFieldTemplate
		* Locale

##Url Redirect
* [View | Manage] Url Redirect
	* Url Redirect [Read | All]
		* URLHandler
		* Locale

##Order
* [View | Manage] Order
	* Order [Read | All]
		* Order
		* OrderAdjustment
		* ORderPayment
		* Country
		* State
		* PaymentTransactionImpl
	* Order Item [Read | All]
		* OrderItem
		* DiscreteOrderItemFeePrice
		* OrderItemAdjustment
		* OrderItemPriceDetailAdjustmentImpl
		* OrderItemPriceDetailImpl
		* BundleOrderItemFeePriceImpl
	* Fulfillment Group [Read | All]
		* FulfillmentGroup
		* FulfillmentGroupAdjustment
		* FulfillmentGroupFeeImpl
		* FulfillmentGroupItemImpl
	* Offer [Read | All]
		* Offer
		* OfferItemCriteria
		* OfferCode

##Customer
* [View | Manage] Customer
	* Customer [Read | All]
		* Customer
		* ChallengeQuestion
		* CustomerAttribute
		* CustomerAddress
		* CustomerPayment
		* CustomerPhone
		* CrossSaleProductImpl

##User
* [View | Manage] User
	* User [Read | All]
		* AdminUser
		* AdminRole
		* AdminPermission

##System Property
* [View | Manage] System Property
	* System Property [Read | All]
		* SystemProperty

##Search Redirect
* [View | Manage] Search Redirect
	* Search Redirect [Read | All]
		* SearchRedirect

##Module Configuration
* [View | Manage] Module Configuration
	* Module Configuration [Read | All]
		* ModuleConfiguration

##Enumeration
* [View | Manage] Enumeration
	* Enumeration [Read | All]
		* DataDrivenEnumeration
		* DataDrivenEnumerationValue

##Translation
* [View | Manage] Translation
	* Translation [Read | All]
		* Translation

##Site Map Generation Configuration
* [View | Manage] Site Map Gen Config
	* Site Map Gen Config [Read | All]
		* SiteMapGeneratorConfiguration
		* SiteMapURLEntry

##Sku
* [View | Manage] Sku
	* Sku [Read | All]
		* Sku

##Currency
* [View | Manage] Currency
	* Currency [Read | All]
		* BroadleafCurrency
