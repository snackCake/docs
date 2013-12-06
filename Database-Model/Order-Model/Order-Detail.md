# Order Detail

###Detailed ERD

[![Order Detail](dataModel/OrderDetailedERD.png)](_img/dataModel/OrderDetailedERD.png)

###Tables

| Table                      | Related Entity | Description                                         |
|:---------------------------|:----------|:----------------------------------------------------|
|BLC\_BUNDLE\_ORDER\_ITEM       | ^[javadoc:org/broadleafcommerce/core/order/domain/BundleOrderItem]      | Contains a group of discrete order items   |
|BLC\_BUND\_ITEM\_FEE\_PRICE     | ^[javadoc:org/broadleafcommerce/core/order/domain/BundleOrderItemFeePrice]      | Contains fee information for a bundle order item  |
|BLC\_DISCRETE\_ORDER\_ITEM     | ^[javadoc:org/broadleafcommerce/core/order/domain/DiscreteOrderItem]      | Contains product, sku, and pricing information for an item on an order  |
|BLC\_DISC\_ITEM\_FEE\_PRICE     | ^[javadoc:org/broadleafcommerce/core/order/domain/DiscreteOrderItemFeePrice]      | Contains fee information for a discrete order item  |
|BLC\_DYN\_DISCRETE\_ORDER\_ITEM | ^[javadoc:org/broadleafcommerce/core/order/domain/DynamicPriceDiscreteOrderItem]      | Contains discrete order item information that is dynamically priced  |
|BLC\_FG\_ADJUSTMENT           | ^[javadoc:org/broadleafcommerce/core/offer/domain/FulfillmentGroupAdjustment]      | Contains offer information and amount applied to a fulfillment group  |
|BLC\_GIFTWRAP\_ORDER\_ITEM     | ^[javadoc:org/broadleafcommerce/core/order/domain/GiftWrapOrderItem]      | Declares which discrete order items are gift-wrapped  |
|BLC\_ORDER                   | ^[javadoc:org/broadleafcommerce/core/order/domain/Order]      | Represents an order in Broadleaf  |
|BLC\_ORDER\_ADJUSTMENT        | [OrderAdjustment.java](OrderAdjustment)      | Contains offer information and amount applied to an order  |
|BLC\_ORDER\_ATTRIBUTE         | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderAttribute]      | Contains arbitrary data about an order  |
|BLC\_ORDER\_ITEM              | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderItem]      | An abstract representation of an item on an order  |
|BLC\_ORDER\_ITEM\_ADD\_ATTR     | n/a      | Contains arbitrary data about a discrete order item  |
|BLC\_ORDER\_ITEM\_ADJUSTMENT   | ^[javadoc:org/broadleafcommerce/core/offer/domain/OrderItemAdjustment]      | Contains offer information and amount applied to an order item  |
|BLC\_ORDER\_ITEM\_ATTRIBUTE    | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderItemAttribute]      | Contains arbitrary data about an order item  |
|BLC\_ITEM\_OFFER\_QUALIFIER    | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderItemQualifier]      | Contains data about order item qualifies  |
|BLC\_ORDER\_ITEM\_PRICE\_DTL    | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderItemPriceDetailImpl]      | Contains order item price detail information  |
|BLC\_ORDER\_ITEM\_DTL\_ADJ      | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderItemPriceDetailAdjustmentImpl]      | Contains order item price detail adjustment information  |
|BLC\_PERSONAL\_MESSAGE        | ^[javadoc:org/broadleafcommerce/core/order/domain/PersonalMessage]      | Contains personal message information (e.g. from, to, message body)   |
|BLC\_ORDER\_MULTISHIP\_OPTION  | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderMultishipOption]      | Represents a given set of options for an OrderItem in an Order in the multiship context  |
|BLC\_ORDER\_OFFER\_CODE\_XREF   | n/a      | Cross-reference from orders to offers  |

###Related Tables

| Table                | Related Entity    | Description                                         |
|:---------------------|:--------------|:----------------------------------------------------|
|BLC\_ADDRESS           | ^[javadoc:org/broadleafcommerce/profile/core/domain/Address]           | Contains address information, e.g. city, state, and postal code  |
|BLC\_CATEGORY          | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Category]          | Represents a category  |
|BLC\_CUSTOMER          | ^[javadoc:org/broadleafcommerce/profile/core/domain/Customer]          | Represents a customer in Broadleaf  |
|BLC\_FULFILLMENT\_OPTION| ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentOption]          | Holds information about a particular fulfillment implementation  |
|BLC\_FULFILLMENT\_GROUP | ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroup]          | Holds fulfillment information about an order  |
|BLC\_OFFER             | ^[javadoc:org/broadleafcommerce/core/offer/domain/Offer]          | Contains adjustment information and rules  |
|BLC\_PRODUCT           | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Product]          | Contains product information  |
|BLC\_PRODUCT\_BUNDLE    | ^[javadoc:org/broadleafcommerce/core/catalog/domain/ProductBundle]          | Contains data about a product bundle, e.g. bundle pricing |
|BLC\_SKU               | ^[javadoc:org/broadleafcommerce/core/catalog/domain/Sku]          | Contains information about an item that can be sold  |
|BLC\_SKU\_BUNDLE\_ITEM   | ^[javadoc:org/broadleafcommerce/core/catalog/domain/SkuBundleItem]          | Contains bundle item metadata  |
|BLC\_CURRENCY                | ^[javadoc:org/broadleafcommerce/common/currency/domain/BroadleafCurrency]      | Contains currency information, such as code and if it's default  |
|BLC\_LOCALE                  | ^[javadoc:org/broadleafcommerce/common/locale/domain/Locale]      | Contains locale information, such as code and if it's default  |
