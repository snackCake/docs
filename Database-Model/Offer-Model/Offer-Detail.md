# Offer Detail



###Detailed ERD

[![Offer Detail](dataModel/OfferDetailedERD.png)](_img/dataModel/OfferDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_OFFER            | ^[javadoc:org/broadleafcommerce/core/offer/domain/Offer]          | Represents an Offer in Broadleaf.  |
|BLC_OFFER_CODE       | ^[javadoc:org/broadleafcommerce/core/offer/domain/OfferCode]          | Represents an Offer Code.  |
|BLC_OFFER_AUDIT      | ^[javadoc:org/broadleafcommerce/core/offer/domain/OfferAudit]          | Represents an Offer Audit.  |
|BLC_OFFER_CANDIDATE_FG_OFFER| ^[javadoc:org/broadleafcommerce/core/offer/domain/CandidateFulfillmentGroupOffer]   | Represents an Offer candidate.  |
|BLC_CANDIDATE_ITEM_OFFER    | ^[javadoc:org/broadleafcommerce/core/offer/domain/CandidateItemOffer]   | Represents an Offer Item candidate.  |
|BLC_CANDIDATE_ORDER_OFFER   | ^[javadoc:org/broadleafcommerce/core/offer/domain/CandidateOrderOffer]   | Represents an Offer Order candidate.  |
|BLC_ADDITIONAL_OFFER_INFO   | -   | Represents additional information for an Offer.  |
|BLC_OFFER_INFO       | ^[javadoc:org/broadleafcommerce/core/offer/domain/OfferInfo]          | Links to the Offer Info fields.  |
|BLC_OFFER_INFO_FIELDS| -          | Represents an Offer Info fields.  |
|BLC_OFFER_RULE       | ^[javadoc:org/broadleafcommerce/core/offer/domain/OfferRule]          | Represents a rule to be applied to an Offer.  |
|BLC_OFFER_RULE_MAP   | -          | Maps an Offer to a Rule.  |
|BLC_OFFER_ITEM_CRITERIA | ^[javadoc:org/broadleafcommerce/core/offer/domain/OfferItemCriteria]       | Represents an Offer item criteria.  |
|BLC_QUAL_CRIT_OFFER_XREF| ^[javadoc:org/broadleafcommerce/core/offer/domain/CriteriaOfferXref]       | Cross reference table that points to an Offer item criteria.  |
|BLC_TAR_CRIT_OFFER_XREF | -       | Cross reference table that points to an Offer target item criteria.  |


###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_FULFILLMENT_GROUP| ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroup]          | Holds fulfillment information about an order.  |
|BLC_ORDER_ITEM       | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderItem]          | An abstract representation of an item on an order.  |
|BLC_ORDER            | ^[javadoc:org/broadleafcommerce/core/order/domain/Order]      | Represents an order in Broadleaf  |
