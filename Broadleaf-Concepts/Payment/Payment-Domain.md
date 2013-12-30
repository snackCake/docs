# Payment Domain

## Payment
`OrderPayment` is how customers indicate how they will pay for their order. Conceptually, a payment is more about what a customer *intends* to do, while a `PaymentTransaction` is what actually happened to that payment. For instance, a customer might say that they want to pay for $20 of the order with a gift card (which would be an `OrderPayment`) but actually decrementing the amount from the gift card would not occur until checkout. **All completed orders should have at least 1 `OrderPayment` and that `OrderPayment` should have at least 1 confirmed and successful transaction**. A few key parts make up a payment:

### Type
Broadleaf has some predefined payment types:

- Credit Card
- Gift Card
- Bank Account
- Check
- Electronic Check
- Wire
- Money Order
- Customer Credit
- Collect on delivery (can also be used for cash)

### Status
##TODO
This is really a convenience property that can be predetermined by looking at the payment 

### Amount
How much the customer is going to pay with this particular payment on a particular order. In the common case, a customer will pay for the entire order total using only a single payment (1 credit card) but you might want to support a customer paying for items with both a credit card and a gift card, or multiple gift cards or even multiple credit cards.

> While the Broadleaf domain does not explicitly prevent you from allowing customers to pay for an order with multiple credit cards, if you do not want to store the credit cards yourself (and forego the PCI requirements) but also still capture multiple cards then this scenario becomes very complicated.

### Billing address
This is normally used for credit cards. Gift cards do not usually require a billing address

### Gateway type
Allows for easy reference to what gateway was used for this particular payment for auditing purposes or in multi-site configurations where 2 sites might use different payment processors. While Broadleaf does not provide any of these types out of the box, this will be used by different payment providers (Braintree, Authorize.net, Cybersource, etc).

### Transactions
A transaction is some sort of modification for a particular payment. For instance, a customer might say that they intend to pay for an order with $20 from a credit card (which would be an `OrderPaymen	t`) but then actually *authorizing* or *capturing* the card would occur within a transaction.

Read more about [[Payment Transactions]]

## Checkout Workflow Activity
In previous versions of Broadleaf, there was a concept of a payment workflow, payment activities and payment modules. The payment workflow was invoked inside of the checkout workflow, coupling those concepts very close together. We have since split payments into a process that should occur *prior to* invoking the checkout workflow. The only part that happens within the checkout workflow is an activity that ensures that the entire order has been paid for and attempts to confirm any unconfirmed transactions.

## Storing Customer Credit Cards
Entities that implement the `Referenced` interface (`GiftCardPayment`, `CreditCardPayment`, `BankAccountPayment`) are designed to be stored in separate database with proper PCI considerations taken into account in order to store customer credit cards. This is not something that we would normally recommend as the PCI auditing process can be difficult and expensive and 99% of the time you don't need it. If the requirement is really to just allow customers to save their credit card information, some external payment gateways have a "vault" feature where the customer credit card still never hits your server.

If you would still like to store sensitive payment information, then you should be utilizing the `SecurePaymentService` to create and store the sensitive data. Under the covers, this will persist the above entities into the `blSecurePU` persistence unit which is separated from all of your other entities. To link the secure payment information to an Order Payment you can use the `referenceNumber` property on both of the entities. This provides a soft link between the insecure OrderPayment entity and the secure Referenced entity to allow you to inspect how the customer actually paid for the order.

For more information about storing credit cards in your own system, see [[Payment Security and PCI Compliance]]