# Order Payment

###Detailed ERD

[![Order Payment](dataModel/OrderPaymentDetailedERD.png)](_img/dataModel/OrderPaymentDetailedERD.png)

###Tables

| Table                        | Related Entity | Description                                         |
|:-----------------------------|:----------|:----------------------------------------------------|
|BLC\_AMOUNT\_ITEM               | [AmountItem.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/payment/domain/AmountItem.html)      | Contains item information for items in a payment  |
|BLC\_BANK\_ACCOUNT\_PAYMENT      | [BankAccountPaymentInfo.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/payment/domain/BankAccountPaymentInfo.html)      | Contains data about a bank account used for payment  |
|BLC\_GIFT\_CARD\_PAYMENT         | [GiftCardPaymentInfo.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/payment/domain/GiftCardPaymentInfo.html)      | Contains data about a gift card used for payment  |
|BLC\_CREDIT\_CARD\_PAYMENT       | [CreditCardPaymentInfo.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/payment/domain/CreditCardPaymentInfo.html)      | Contains information about a credit card used for payment  |
|BLC\_ORDER\_PAYMENT             | [PaymentInfo.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/payment/domain/PaymentInfo.html)      | Contains payment information for an order  |
|BLC\_ORDER\_PAYMENT\_DETAILS     | [PaymentInfoDetail.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/payment/domain/PaymentInfoDetail.html)      | Contains detailed payment information for an order  |
|BLC\_PAYINFO\_ADDITIONAL\_FIELDS | n/a      | Contains arbitrary payment data  |
|BLC\_PAYMENT\_ADDITIONAL\_FIELDS | n/a      | Contains arbitrary payment data for the payment response  |
|BLC\_PAYMENT\_LOG               | [PaymentLog.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/payment/domain/PaymentLog.html)      | Contains summary information for a payment instance  |
|BLC\_PAYMENT\_RESPONSE\_ITEM     | [PaymentResponseItem.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/payment/domain/PaymentResponseItem.html)      | Contains payment response information from payment gateway  |

###Related Tables

| Table       | Related Entity   | Description                                         |
|:------------|:-----------------|:----------------------------------------------------|
|BLC\_ADDRESS  | [Address.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Address.html)          | Contains address information, e.g. city, state, and postal code  |
|BLC\_CUSTOMER | [Customer.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Customer.html)          | Represents a customer in Broadleaf  |
|BLC\_ORDER    | [Order.java](http://javadoc.broadleafcommerce.org/current/framework/org/broadleafcommerce/core/order/domain/Order.html)          | Represents an order in Broadleaf  |
|BLC\_PHONE    | [Phone.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Phone.html)          | Represents a phone in Broadleaf  |
|BLC\_CURRENCY | [BroadleafCurrency.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/currency/domain/BroadleafCurrency.html)      | Contains currency information, such as code and if it's default  |
|BLC\_CUSTOMER\_PAYMENT | [CustomerPayment.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/CustomerPayment.html)  | Contains customer payment information.    |

