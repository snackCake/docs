# Order Payment

###Detailed ERD

[![Order Payment](dataModel/OrderPaymentDetailedERD.png)](_img/dataModel/OrderPaymentDetailedERD.png)

###Tables

| Table                        | Related Entity | Description                                         |
|:-----------------------------|:----------|:----------------------------------------------------|
|BLC\_AMOUNT\_ITEM               | ^[javadoc:org/broadleafcommerce/core/payment/domain/AmountItem]      | Contains item information for items in a payment  |
|BLC\_BANK\_ACCOUNT\_PAYMENT      | ^[javadoc:org/broadleafcommerce/core/payment/domain/BankAccountPaymentInfo]      | Contains data about a bank account used for payment  |
|BLC\_GIFT\_CARD\_PAYMENT         | ^[javadoc:org/broadleafcommerce/core/payment/domain/GiftCardPaymentInfo]      | Contains data about a gift card used for payment  |
|BLC\_CREDIT\_CARD\_PAYMENT       | ^[javadoc:org/broadleafcommerce/core/payment/domain/CreditCardPaymentInfo]      | Contains information about a credit card used for payment  |
|BLC\_ORDER\_PAYMENT             | ^[javadoc:org/broadleafcommerce/core/payment/domain/PaymentInfo]      | Contains payment information for an order  |
|BLC\_ORDER\_PAYMENT\_DETAILS     | ^[javadoc:org/broadleafcommerce/core/payment/domain/PaymentInfoDetail]      | Contains detailed payment information for an order  |
|BLC\_PAYINFO\_ADDITIONAL\_FIELDS | n/a      | Contains arbitrary payment data  |
|BLC\_PAYMENT\_ADDITIONAL\_FIELDS | n/a      | Contains arbitrary payment data for the payment response  |
|BLC\_PAYMENT\_LOG               | ^[javadoc:org/broadleafcommerce/core/payment/domain/PaymentLog]      | Contains summary information for a payment instance  |
|BLC\_PAYMENT\_RESPONSE\_ITEM     | ^[javadoc:org/broadleafcommerce/core/payment/domain/PaymentResponseItem]      | Contains payment response information from payment gateway  |

###Related Tables

| Table       | Related Entity   | Description                                         |
|:------------|:-----------------|:----------------------------------------------------|
|BLC\_ADDRESS  | ^[javadoc:org/broadleafcommerce/profile/core/domain/Address]          | Contains address information, e.g. city, state, and postal code  |
|BLC\_CUSTOMER | ^[javadoc:org/broadleafcommerce/profile/core/domain/Customer]          | Represents a customer in Broadleaf  |
|BLC\_ORDER    | ^[javadoc:org/broadleafcommerce/core/order/domain/Order]          | Represents an order in Broadleaf  |
|BLC\_PHONE    | ^[javadoc:org/broadleafcommerce/profile/core/domain/Phone]          | Represents a phone in Broadleaf  |
|BLC\_CURRENCY | ^[javadoc:org/broadleafcommerce/common/currency/domain/BroadleafCurrency]      | Contains currency information, such as code and if it's default  |
|BLC\_CUSTOMER\_PAYMENT | ^[javadoc:org/broadleafcommerce/profile/core/domain/CustomerPayment]  | Contains customer payment information.    |

