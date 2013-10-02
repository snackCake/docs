# Offer Detail



###Detailed ERD

[![Offer Detail](dataModel/OfferDetailedERD.png)](_img/dataModel/OfferDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_OFFER            | [Offer.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/offer/domain/Offer.html)          | Represents an Offer in Broadleaf.  |
|BLC\_OFFER\_CODE       | [OfferCode.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/offer/domain/OfferCode.html)          | Represents an Offer Code.  |
|BLC\_OFFER\_AUDIT      | [OfferAudit.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/offer/domain/OfferAudit.html)          | Represents an Offer Audit.  |
|BLC\_OFFER\_CANDIDATE\_FG\_OFFER| [CandidateFulfillmentGroupOffer.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/offer/domain/CandidateFulfillmentGroupOffer.html)   | Represents an Offer candidate.  |
|BLC\_CANDIDATE\_ITEM\_OFFER    | [CandidateItemOffer.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/offer/domain/CandidateItemOffer.html)   | Represents an Offer Item candidate.  |
|BLC\_CANDIDATE\_ORDER\_OFFER   | [CandidateOrderOffer.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/offer/domain/CandidateOrderOffer.html)   | Represents an Offer Order candidate.  |
|BLC\_ADDITIONAL\_OFFER\_INFO   | -   | Represents additional information for an Offer.  |
|BLC\_OFFER\_INFO       | [OfferInfo.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/offer/domain/OfferInfo.html)          | Links to the Offer Info fields.  |
|BLC\_OFFER\_INFO\_FIELDS| -          | Represents an Offer Info fields.  |
|BLC\_OFFER\_RULE       | [OfferRule](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/offer/domain/OfferRule.html)          | Represents a rule to be applied to an Offer.  |
|BLC\_OFFER\_RULE\_MAP   | -          | Maps an Offer to a Rule.  |
|BLC\_OFFER\_ITEM\_CRITERIA | [OfferItemCriteria.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/offer/domain/OfferItemCriteria.html)       | Represents an Offer item criteria.  |
|BLC\_QUAL\_CRIT\_OFFER\_XREF| [CriteriaOfferXref.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/offer/domain/CriteriaOfferXref.html)       | Cross reference table that points to an Offer item criteria.  |
|BLC\_TAR\_CRIT\_OFFER\_XREF | -       | Cross reference table that points to an Offer target item criteria.  |


###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_FULFILLMENT\_GROUP| [FulfillmentGroup.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/FulfillmentGroup.html)          | Holds fulfillment information about an order.  |
|BLC\_ORDER\_ITEM       | [OrderItem.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/OrderItem.html)          | An abstract representation of an item on an order.  |
|BLC\_ORDER            | [Order.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/Order.html)      | Represents an order in Broadleaf  |
