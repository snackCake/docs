# Order Payment

###Detailed ERD

[![Order Payment](dataModel/OrderPaymentDetailedERD.png)](_img/dataModel/OrderPaymentDetailedERD.png)

###Tables

| Table                        | Related Entity | Description                                         |
|:-----------------------------|:----------|:----------------------------------------------------|
|BLC_AMOUNT_ITEM               | ^[javadoc:org/broadleafcommerce/core/payment/domain/AmountItem]      | Contains item information for items in a payment  |
|BLC_BANK_ACCOUNT_PAYMENT      | ^[javadoc:org/broadleafcommerce/core/payment/domain/BankAccountPaymentInfo]      | Contains data about a bank account used for payment  |
|BLC_GIFT_CARD_PAYMENT         | ^[javadoc:org/broadleafcommerce/core/payment/domain/GiftCardPaymentInfo]      | Contains data about a gift card used for payment  |
|BLC_CREDIT_CARD_PAYMENT       | ^[javadoc:org/broadleafcommerce/core/payment/domain/CreditCardPaymentInfo]      | Contains information about a credit card used for payment  |
|BLC_ORDER_PAYMENT             | ^[javadoc:org/broadleafcommerce/core/payment/domain/PaymentInfo]      | Contains payment information for an order  |
|BLC_PAYINFO_ADDITIONAL_FIELDS | n/a      | Contains arbitrary payment data  |
|BLC_PAYMENT_ADDITIONAL_FIELDS | n/a      | Contains arbitrary payment data for the payment response  |
|BLC_PAYMENT_LOG               | ^[javadoc:org/broadleafcommerce/core/payment/domain/PaymentLog]      | Contains summary information for a payment instance  |
|BLC_PAYMENT_RESPONSE_ITEM     | ^[javadoc:org/broadleafcommerce/core/payment/domain/PaymentResponseItem]      | Contains payment response information from payment gateway  |

###Related Tables

| Table       | Related Entity   | Description                                         |
|:------------|:-----------------|:----------------------------------------------------|
|BLC_ADDRESS  | ^[javadoc:org/broadleafcommerce/profile/core/domain/Address]          | Contains address information, e.g. city, state, and postal code  |
|BLC_CUSTOMER | ^[javadoc:org/broadleafcommerce/profile/core/domain/Customer]          | Represents a customer in Broadleaf  |
|BLC_ORDER    | ^[javadoc:org/broadleafcommerce/core/order/domain/Order]          | Represents an order in Broadleaf  |
|BLC_PHONE    | ^[javadoc:org/broadleafcommerce/profile/core/domain/Phone]          | Represents a phone in Broadleaf  |
|BLC_CURRENCY                | ^[javadoc:org/broadleafcommerce/common/currency/domain/BroadleafCurrency]      | Contains currency information, such as code and if it's default  |
