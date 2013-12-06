# Catalog Reviews



###Detailed ERD

[![Catalog Reviews Detail](dataModel/CatalogReviewsDetailedERD.png)](_img/dataModel/CatalogReviewsDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_REVIEW\_DETAIL    | ^[javadoc:org/broadleafcommerce/core/rating/domain/ReviewDetail]          | Represents a review.  |
|BLC\_REVIEW\_FEEDBACK  | ^[javadoc:org/broadleafcommerce/core/rating/domain/ReviewFeedback]          | Represents a the feedback for a review.  |
|BLC\_RATING\_DETAIL    | ^[javadoc:org/broadleafcommerce/core/rating/domain/RatingDetail]          | Represents the detail of a rating.  |
|BLC\_RATING\_SUMMARY   | ^[javadoc:org/broadleafcommerce/core/rating/domain/RatingSummary]          | Represents the summary of a rating.  |



###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_CUSTOMER         | ^[javadoc:org/broadleafcommerce/profile/core/domain/Customer]          | Represents a customer in Broadleaf Commerce.  |
