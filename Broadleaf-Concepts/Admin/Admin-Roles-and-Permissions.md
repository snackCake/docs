# Admin Security

The Broadleaf Commerce Admin provides the ability to granularly control the access rights for admin users.

## Definitions

### Admin User
Represents a user with a login and password to access admin functionality.     A user can be assigned one-or-more Roles as well as one-or-more Permissions.  

### Role
A role represents a group of Permissions.    Generally, a user is assigned a role like "Content Editor".    The role would be setup to contain all of the permissions necessary to complete that role.

### Permission
A permission represents a privilige to do some functionality within the admin.    Examples could include "View Customers" or "Manage Products".    While this basic permission model is easy to understand, Broadleaf provides a couple of additional technical details that are important for those adding custom permissions and capabilities to understand that are discussed below.

## How Permissions Relate To Menu Items
The menu items that an Admin User sees are related to the permissions that they have.

For example, consider the following SQL that was pulled from the `load_admin_menu.sql` file.

```sql
-- Here is SQL the defines permissions for Viewing and Maintaining categories.   
-- Note:  There is more to this ... see "more details about permissions" below ...
INSERT INTO BLC_ADMIN_PERMISSION (ADMIN_PERMISSION_ID, DESCRIPTION, NAME, PERMISSION_TYPE, IS_FRIENDLY)
  VALUES (-100,'View Categories','PERMISSION_CATEGORY', 'READ', TRUE);

INSERT INTO BLC_ADMIN_PERMISSION (ADMIN_PERMISSION_ID, DESCRIPTION, NAME, PERMISSION_TYPE, IS_FRIENDLY) 
  VALUES (-101,'Maintain Categories','PERMISSION_CATEGORY', 'ALL', TRUE);


-- The following SQL inserts an Admin Module which represent the top level menu items like 'Content', 'Catalog'
INSERT INTO BLC_ADMIN_MODULE (ADMIN_MODULE_ID, NAME, MODULE_KEY, ICON, DISPLAY_ORDER) 
  VALUES (-1,'Catalog','BLCMerchandising', 'icon-barcode', 100);

-- The Menu Items (Sections) are added to a module with SQL like the following
INSERT INTO BLC_ADMIN_SECTION (ADMIN_SECTION_ID, DISPLAY_ORDER, ADMIN_MODULE_ID, NAME, SECTION_KEY, URL, CEILING_ENTITY)   VALUES (-1, 1000, -1, 'Category', 'Category', '/category', 'org.broadleafcommerce.core.catalog.domain.Category');

-- Finally, the section is associated with permissions that allow the user to see that menu item.
-- The example below associates the Category Menu Item with permissions with ID -100 or -101
INSERT INTO BLC_ADMIN_SEC_PERM_XREF (ADMIN_SECTION_ID, ADMIN_PERMISSION_ID) VALUES (-1,-100);
INSERT INTO BLC_ADMIN_SEC_PERM_XREF (ADMIN_SECTION_ID, ADMIN_PERMISSION_ID) VALUES (-1,-101);

```

## More about Permissions
The Broadleaf Admin provides a lot of functionality with a very tight security model.   When a user is given a "Permission", the system needs to understand what underlying Entities (think tables) that they can view or modify.  

This can sometimes be quite involved.   For example, consider a user that can edit products but not categories.   In this example, the user would need "Edit" access to Products but "View" access to categories since one of the properties on a product is it's default category.

To handle this (and much more complex scenarios), Broadleaf has the following logical permission model.

[![Admin Permissions Logical Model](admin-permissions.png)](_img/admin-permissions.png)


Some notes about the above diagram.    
- Both "Friendly" and "Child" Permissions live in the same BLC table.
- Child Permissions have a permissionType which can be READ, ALL, DELETE, UPDATE, CREATE.   Broadleaf only uses READ and ALL but the others are available for custom solutions.
- Each child permission is associated with 1 or more EntityPermissions
- EntityPermissions directly relate to the underlying JPA objects (like CategoryImpl.java).


## Permission and role structure
For the lastest on this, consult your admin or look in the actual permission SQL files.


Below you will find each of the out of box menu items with it's friendly permission, child permissions, and entity permissions in the following structure.

###Section
* Friendly Permission & Type (View or Manage)
	* Permission & Type (Read or All)
		* Entity Permission

----- 

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

###Product
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

###Product Option
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

###Offer
* [View | Manage] Offer
	* Offer [Read | All]
		* Offer
		* OfferItemCriteria
		* OfferCode

###Page
* [View | Manage] Page
	* Page [Read | All]
		* Page
		* PageTemplate
		* PageItemCriteria
		* Locale

###Asset
* [View | Manage] Asset
	* Asset [Read | All]
		* StaticAsset
		* StaticAssetFolder

###Structured Content
* [View | Manage] Structured Content
	* Structured Content [Read | All]
		* StructuredContent
		* StructuredContentType
		* StructuredContentItemCriteria
		* StructuredContentFieldTemplate
		* Locale

###Url Redirect
* [View | Manage] Url Redirect
	* Url Redirect [Read | All]
		* URLHandler
		* Locale

###Order
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

###Customer
* [View | Manage] Customer
	* Customer [Read | All]
		* Customer
		* ChallengeQuestion
		* CustomerAttribute
		* CustomerAddress
		* CustomerPayment
		* CustomerPhone
		* CrossSaleProductImpl

###User
* [View | Manage] User
	* User [Read | All]
		* AdminUser
		* AdminRole
		* AdminPermission

###System Property
* [View | Manage] System Property
	* System Property [Read | All]
		* SystemProperty

###Search Redirect
* [View | Manage] Search Redirect
	* Search Redirect [Read | All]
		* SearchRedirect

###Module Configuration
* [View | Manage] Module Configuration
	* Module Configuration [Read | All]
		* ModuleConfiguration

##Enumeration
* [View | Manage] Enumeration
	* Enumeration [Read | All]
		* DataDrivenEnumeration
		* DataDrivenEnumerationValue

###Translation
* [View | Manage] Translation
	* Translation [Read | All]
		* Translation

###Site Map Generation Configuration
* [View | Manage] Site Map Gen Config
	* Site Map Gen Config [Read | All]
		* SiteMapGeneratorConfiguration
		* SiteMapURLEntry

###Sku
* [View | Manage] Sku
	* Sku [Read | All]
		* Sku

###Currency
* [View | Manage] Currency
	* Currency [Read | All]
		* BroadleafCurrency
