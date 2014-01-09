# Payment and Pricing Activities

> Note - Please familiarize yourself with [[Workflows and Activities]] before proceeding.

## Overview

Since, the Payment Workflow has been deprecated and decoupled from the Checkout Workflow, there are only a few
activities that relate to payment. Let's take a look at them:

### `blValidateAndConfirmPaymentActivity`

This activity is executed in the `blCheckoutWorkflow` and is responsible for verifying that there is enough payment on the order via the `successful` amount on `PaymentTransactionType.AUTHORIZE` and `PaymentTransactionType.AUTHORIZE_AND_CAPTURE` transactions. This will also confirm any `PaymentTransactionType.UNCONFIRMED` transactions that exist on an `OrderPayment`.

If there is an exception (either in this activity or later downstream) the confirmed payments are rolled back via `ConfirmPaymentsRollbackHandler`

The `ConfirmPaymentsRollbackHandler` rolls back all payments that have been processed or were confirmed by the `ValidateAndConfirmPaymentActivity` using the Gateway implementations `PaymentGatewayRollbackService` for that particular transaction.

### `blAdjustOrderPaymentsActivity`

This activity is executed in the `blPricingWorkflow` as a way to reconcile `PaymentTransactionType.UNCONFIRMED` transactions for `PaymentType.THIRD_PARTY_ACCOUNT`. This activity is responsible for adjusting any of the order payments that have already been applied to the order. This happens when order payments have been applied to the order but the order total has changed. In the case of a hosted gateway solution like PayPal Express Checkout, the order payment is created when the customer redirects to the Review Order Page (Checkout page) and the user selects a shipping method which may affect the order total. Since the Hosted Order payment is unconfirmed, we need to adjust the amount on this order payment before we complete checkout and confirm the payment with PayPal again. For this default implementation, the remaining difference is added to the the Order Payment of `PaymentType.THIRD_PARTY_ACCOUNT`.
