# Catalog Reviews



###Detailed ERD

[![Catalog Reviews Detail](dataModel/CatalogReviewsDetailedERD.png)](_img/dataModel/CatalogReviewsDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_REVIEW_DETAIL    | ^[javadoc:org/broadleafcommerce/core/rating/domain/ReviewDetail]          | Represents a review.  |
|BLC_REVIEW_FEEDBACK  | ^[javadoc:org/broadleafcommerce/core/rating/domain/ReviewFeedback]          | Represents a the feedback for a review.  |
|BLC_RATING_DETAIL    | ^[javadoc:org/broadleafcommerce/core/rating/domain/RatingDetail]          | Represents the detail of a rating.  |
|BLC_RATING_SUMMARY   | ^[javadoc:org/broadleafcommerce/core/rating/domain/RatingSummary]          | Represents the summary of a rating.  |



###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_CUSTOMER         | ^[javadoc:org/broadleafcommerce/profile/core/domain/Customer]          | Represents a customer in Broadleaf Commerce.  |
