# Order Detail

###Detailed ERD

[![Order Detail](dataModel/OrderDetailedERD.png)](_img/dataModel/OrderDetailedERD.png)

###Tables

| Table                      | Related Entity | Description                                         |
|:---------------------------|:----------|:----------------------------------------------------|
|BLC_BUNDLE_ORDER_ITEM       | ^[javadoc:org/broadleafcommerce/core/order/domain/BundleOrderItem]      | Contains a group of discrete order items   |
|BLC_BUND_ITEM_FEE_PRICE     | ^[javadoc:org/broadleafcommerce/core/order/domain/BundleOrderItemFeePrice]      | Contains fee information for a bundle order item  |
|BLC_DISCRETE_ORDER_ITEM     | ^[javadoc:org/broadleafcommerce/core/order/domain/DiscreteOrderItem]      | Contains product, sku, and pricing information for an item on an order  |
|BLC_DISC_ITEM_FEE_PRICE     | ^[javadoc:org/broadleafcommerce/core/order/domain/DiscreteOrderItemFeePrice]      | Contains fee information for a discrete order item  |
|BLC_DYN_DISCRETE_ORDER_ITEM | ^[javadoc:org/broadleafcommerce/core/order/domain/DynamicPriceDiscreteOrderItem]      | Contains discrete order item information that is dynamically priced  |
|BLC_FG_ADJUSTMENT           | ^[javadoc:org/broadleafcommerce/core/offer/domain/FulfillmentGroupAdjustment]      | Contains offer information and amount applied to a fulfillment group  |
|BLC_GIFTWRAP_ORDER_ITEM     | ^[javadoc:org/broadleafcommerce/core/order/domain/GiftWrapOrderItem]      | Declares which discrete order items are gift-wrapped  |
|BLC_ORDER                   | ^[javadoc:org/broadleafcommerce/core/order/domain/Order]      | Represents an order in Broadleaf  |
|BLC_ORDER_ADJUSTMENT        | [OrderAdjustment.java](OrderAdjustment)      | Contains offer information and amount applied to an order  |
|BLC_ORDER_ATTRIBUTE         | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderAttribute]      | Contains arbitrary data about an order  |
|BLC_ORDER_ITEM              | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderItem]      | An abstract representation of an item on an order  |
|BLC_ORDER_ITEM_ADD_ATTR     | n/a      | Contains arbitrary data about a discrete order item  |
|BLC_ORDER_ITEM_ADJUSTMENT   | ^[javadoc:org/broadleafcommerce/core/offer/domain/OrderItemAdjustment]      | Contains offer information and amount applied to an order item  |
|BLC_ORDER_ITEM_ATTRIBUTE    | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderItemAttribute]      | Contains arbitrary data about an order item  |
|BLC_PERSONAL_MESSAGE        | ^[javadoc:org/broadleafcommerce/core/order/domain/PersonalMessage]      | Contains personal message information (e.g. from, to, message body)   |
|BLC_ORDER_MULTISHIP_OPTION  | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderMultishipOption]      | Represents a given set of options for an OrderItem in an Order in the multiship context  |
|BLC_ORDER_OFFER_CODE_XREF   | n/a      | Cross-reference from orders to offers  |

###Related Tables

| Table                | Related Entity    | Description                                         |
|:---------------------|:--------------|:----------------------------------------------------|
|BLC_ADDRESS           | ^[javadoc:org/broadleafcommerce/profile/core/domain/Address]           | Contains address information, e.g. city, state, and postal code  |
|BLC_CATEGORY          | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Category]          | Represents a category  |
|BLC_CUSTOMER          | ^[javadoc:org/broadleafcommerce/profile/core/domain/Customer]          | Represents a customer in Broadleaf  |
|BLC_FULFILLMENT_OPTION| ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentOption]          | Holds information about a particular fulfillment implementation  |
|BLC_FULFILLMENT_GROUP | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroup]          | Holds fulfillment information about an order  |
|BLC_OFFER             | ^[javadoc:org/broadleafcommerce/core/offer/domain/Offer]          | Contains adjustment information and rules  |
|BLC_PRODUCT           | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Product]          | Contains product information  |
|BLC_PRODUCT_BUNDLE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductBundle]          | Contains data about a product bundle, e.g. bundle pricing |
|BLC_SKU               | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Sku]          | Contains information about an item that can be sold  |
|BLC_SKU_BUNDLE_ITEM   | ^[javadoc:org/broadleafcommerce/core/catalog/domain/SkuBundleItem]          | Contains bundle item metadata  |
