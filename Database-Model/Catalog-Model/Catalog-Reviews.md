# Catalog Reviews



###Detailed ERD

[![Catalog Reviews Detail](dataModel/CatalogReviewsDetailedERD.png)](_img/dataModel/CatalogReviewsDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_REVIEW\_DETAIL    | [ReviewDetail.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/rating/domain/ReviewDetail.html)          | Represents a review.  |
|BLC\_REVIEW\_FEEDBACK  | [ReviewFeedback.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/rating/domain/ReviewFeedback.html)          | Represents a the feedback for a review.  |
|BLC\_RATING\_DETAIL    | [RatingDetail.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/rating/domain/RatingDetail.html)          | Represents the detail of a rating.  |
|BLC\_RATING\_SUMMARY   | [RatingSummary.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/rating/domain/RatingSummary.html)          | Represents the summary of a rating.  |



###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_CUSTOMER         | [Customer.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Customer.html)          | Represents a customer in Broadleaf Commerce.  |
