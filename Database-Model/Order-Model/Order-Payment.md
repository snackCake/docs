# Order Payment

###Detailed ERD

[![Order Payment](dataModel/OrderPaymentDetailedERD.png)](_img/dataModel/OrderPaymentDetailedERD.png)

###Tables

| Table                        | Related Entity | Description                                         |
|:-----------------------------|:----------|:----------------------------------------------------|
|BLC\_BANK\_ACCOUNT\_PAYMENT      | ^[javadoc:org/broadleafcommerce/core/payment/domain/secure/BankAccountPayment]      | Contains data about a bank account used for payment  |
|BLC\_GIFT\_CARD\_PAYMENT         | ^[javadoc:org/broadleafcommerce/core/payment/domain/secure/GiftCardPayment]      | Contains data about a gift card used for payment  |
|BLC\_CREDIT\_CARD\_PAYMENT       | ^[javadoc:org/broadleafcommerce/core/payment/domain/secure/CreditCardPayment]      | Contains information about a credit card used for payment  |
|BLC\_ORDER\_PAYMENT             | ^[javadoc:org/broadleafcommerce/core/payment/domain/OrderPayment]      | Contains payment information for an order  |
|BLC\_ORDER\_PAYMENT\_TRANSACTION     | ^[javadoc:org/broadleafcommerce/core/payment/domain/PaymentTransaction]      | Contains transaction information related to an Order Payment  |
|BLC\_TRANS\_ADDITNL\_FIELDS | n/a      | Contains important Gateway Specific Payment Data for a particular Payment Transaction |
|BLC\_PAYMENT\_LOG               | ^[javadoc:org/broadleafcommerce/core/payment/domain/PaymentLog]      | Contains summary information for a payment instance  |

###Related Tables

| Table       | Related Entity   | Description                                         |
|:------------|:-----------------|:----------------------------------------------------|
|BLC\_ADDRESS  | ^[javadoc:org/broadleafcommerce/profile/core/domain/Address]          | Contains address information, e.g. city, state, and postal code  |
|BLC\_CUSTOMER | ^[javadoc:org/broadleafcommerce/profile/core/domain/Customer]          | Represents a customer in Broadleaf  |
|BLC\_ORDER    | ^[javadoc:org/broadleafcommerce/core/order/domain/Order]          | Represents an order in Broadleaf  |
|BLC\_PHONE    | ^[javadoc:org/broadleafcommerce/profile/core/domain/Phone]          | Represents a phone in Broadleaf  |
|BLC\_CURRENCY | ^[javadoc:org/broadleafcommerce/common/currency/domain/BroadleafCurrency]      | Contains currency information, such as code and if it's default  |
