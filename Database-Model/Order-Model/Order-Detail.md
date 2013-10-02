# Order Detail

###Detailed ERD

[![Order Detail](dataModel/OrderDetailedERD.png)](_img/dataModel/OrderDetailedERD.png)

###Tables

| Table                      | Related Entity | Description                                         |
|:---------------------------|:----------|:----------------------------------------------------|
|BLC\_BUNDLE\_ORDER\_ITEM       | [BundleOrderItem.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/BundleOrderItem.html)      | Contains a group of discrete order items   |
|BLC\_BUND\_ITEM\_FEE\_PRICE     | [BundleOrderItemFeePrice.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/BundleOrderItemFeePrice.html)      | Contains fee information for a bundle order item  |
|BLC\_DISCRETE\_ORDER\_ITEM     | [DiscreteOrderItem.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/DiscreteOrderItem.html)      | Contains product, sku, and pricing information for an item on an order  |
|BLC\_DISC\_ITEM\_FEE\_PRICE     | [DiscreteOrderItemFeePrice.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/DiscreteOrderItemFeePrice.html)      | Contains fee information for a discrete order item  |
|BLC\_DYN\_DISCRETE\_ORDER\_ITEM | [DynamicPriceDiscreteOrderItem.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/DynamicPriceDiscreteOrderItem.html)      | Contains discrete order item information that is dynamically priced  |
|BLC\_FG\_ADJUSTMENT           | [FulfillmentGroupAdjustment.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/offer/domain/FulfillmentGroupAdjustment.html)      | Contains offer information and amount applied to a fulfillment group  |
|BLC\_GIFTWRAP\_ORDER\_ITEM     | [GiftWrapOrderItem.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/GiftWrapOrderItem.html)      | Declares which discrete order items are gift-wrapped  |
|BLC\_ORDER                   | [Order.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/Order.html)      | Represents an order in Broadleaf  |
|BLC\_ORDER\_ADJUSTMENT        | [OrderAdjustment.java](OrderAdjustment)      | Contains offer information and amount applied to an order  |
|BLC\_ORDER\_ATTRIBUTE         | [OrderAttribute.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/OrderAttribute.html)      | Contains arbitrary data about an order  |
|BLC\_ORDER\_ITEM              | [OrderItem.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/OrderItem.html)      | An abstract representation of an item on an order  |
|BLC\_ORDER\_ITEM\_ADD\_ATTR     | n/a      | Contains arbitrary data about a discrete order item  |
|BLC\_ORDER\_ITEM\_ADJUSTMENT   | [OrderItemAdjustment.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/offer/domain/OrderItemAdjustment.html)      | Contains offer information and amount applied to an order item  |
|BLC\_ORDER\_ITEM\_ATTRIBUTE    | [OrderItemAttribute.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/OrderItemAttribute.html)      | Contains arbitrary data about an order item  |
|BLC\_ITEM\_OFFER\_QUALIFIER    | [OrderItemQualifier.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/OrderItemQualifier.html)      | Contains data about order item qualifies  |
|BLC\_ORDER\_ITEM\_PRICE\_DTL    | [OrderItemPriceDetailImpl.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/OrderItemPriceDetailImpl.html)      | Contains order item price detail information  |
|BLC\_ORDER\_ITEM\_DTL\_ADJ      | [OrderItemPriceDetailAdjustmentImpl.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/OrderItemPriceDetailAdjustmentImpl.html)      | Contains order item price detail adjustment information  |
|BLC\_PERSONAL\_MESSAGE        | [PersonalMessage.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/PersonalMessage.html)      | Contains personal message information (e.g. from, to, message body)   |
|BLC\_ORDER\_MULTISHIP\_OPTION  | [OrderMultishipOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/OrderMultishipOption.html)      | Represents a given set of options for an OrderItem in an Order in the multiship context  |
|BLC\_ORDER\_OFFER\_CODE\_XREF   | n/a      | Cross-reference from orders to offers  |

###Related Tables

| Table                | Related Entity    | Description                                         |
|:---------------------|:--------------|:----------------------------------------------------|
|BLC\_ADDRESS           | [Address.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Address.html)           | Contains address information, e.g. city, state, and postal code  |
|BLC\_CATEGORY          | [Category.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Category.html)          | Represents a category  |
|BLC\_CUSTOMER          | [Customer.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Customer.html)          | Represents a customer in Broadleaf  |
|BLC\_FULFILLMENT\_OPTION| [FulfillmentOption.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/FulfillmentOption.html)          | Holds information about a particular fulfillment implementation  |
|BLC\_FULFILLMENT\_GROUP | [FulfillmentGroup](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/FulfillmentGroup.html)          | Holds fulfillment information about an order  |
|BLC\_OFFER             | [Offer.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/offer/domain/Offer.html)          | Contains adjustment information and rules  |
|BLC\_PRODUCT           | [Product.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Product.html)          | Contains product information  |
|BLC\_PRODUCT\_BUNDLE    | [ProductBundle.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/ProductBundle.html)          | Contains data about a product bundle, e.g. bundle pricing |
|BLC\_SKU               | [Sku.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/Sku.html)          | Contains information about an item that can be sold  |
|BLC\_SKU\_BUNDLE\_ITEM   | [SkuBundleItem.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/catalog/domain/SkuBundleItem.html)          | Contains bundle item metadata  |
|BLC\_CURRENCY                | [BroadleafCurrency.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/currency/domain/BroadleafCurrency.html)      | Contains currency information, such as code and if it's default  |
|BLC\_LOCALE                  | [Locale.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/locale/domain/Locale.html)      | Contains locale information, such as code and if it's default  |
