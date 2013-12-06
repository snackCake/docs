# Offer Detail



###Detailed ERD

[![Offer Detail](dataModel/OfferDetailedERD.png)](_img/dataModel/OfferDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_OFFER            | ^[javadoc:org/broadleafcommerce/core/offer/domain/Offer]          | Represents an Offer in Broadleaf.  |
|BLC\_OFFER\_CODE       | ^[javadoc:org/broadleafcommerce/core/offer/domain/OfferCode]          | Represents an Offer Code.  |
|BLC\_OFFER\_AUDIT      | ^[javadoc:org/broadleafcommerce/core/offer/domain/OfferAudit]          | Represents an Offer Audit.  |
|BLC\_OFFER\_CANDIDATE\_FG\_OFFER| ^[javadoc:org/broadleafcommerce/core/offer/domain/CandidateFulfillmentGroupOffer]   | Represents an Offer candidate.  |
|BLC\_CANDIDATE\_ITEM\_OFFER    | ^[javadoc:org/broadleafcommerce/core/offer/domain/CandidateItemOffer]   | Represents an Offer Item candidate.  |
|BLC\_CANDIDATE\_ORDER\_OFFER   | ^[javadoc:org/broadleafcommerce/core/offer/domain/CandidateOrderOffer]   | Represents an Offer Order candidate.  |
|BLC\_ADDITIONAL\_OFFER\_INFO   | -   | Represents additional information for an Offer.  |
|BLC\_OFFER\_INFO       | ^[javadoc:org/broadleafcommerce/core/offer/domain/OfferInfo]          | Links to the Offer Info fields.  |
|BLC\_OFFER\_INFO\_FIELDS| -          | Represents an Offer Info fields.  |
|BLC\_OFFER\_RULE       | ^[javadoc:org/broadleafcommerce/core/offer/domain/OfferRule]          | Represents a rule to be applied to an Offer.  |
|BLC\_OFFER\_RULE\_MAP   | -          | Maps an Offer to a Rule.  |
|BLC\_OFFER\_ITEM\_CRITERIA | ^[javadoc:org/broadleafcommerce/core/offer/domain/OfferItemCriteria]       | Represents an Offer item criteria.  |
|BLC\_QUAL\_CRIT\_OFFER\_XREF| ^[javadoc:org/broadleafcommerce/core/offer/domain/CriteriaOfferXref]       | Cross reference table that points to an Offer item criteria.  |
|BLC\_TAR\_CRIT\_OFFER\_XREF | -       | Cross reference table that points to an Offer target item criteria.  |


###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_FULFILLMENT\_GROUP| ^[javadoc:org/broadleafcommerce/core/order/domain/FulfillmentGroup]          | Holds fulfillment information about an order.  |
|BLC\_ORDER\_ITEM       | ^[javadoc:org/broadleafcommerce/core/order/domain/OrderItem]          | An abstract representation of an item on an order.  |
|BLC\_ORDER            | ^[javadoc:org/broadleafcommerce/core/order/domain/Order]      | Represents an order in Broadleaf  |
