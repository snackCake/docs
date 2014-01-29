## Handling Payment Rollback Scenarios

If for some reason, the payments need to be rolled back, all the Payment Gateway integrations provided by Broadleaf contain an implementation of `PaymentGatewayRollbackService` that will by default, create a compensating transaction for any payment that was already exectued.

The way that the `blCheckoutWorkflow` works, involves executing several activities to complete an order and mark it as `SUBMITTED`. Any of those activities can throw an exception and cause a rollback. A real world example may be a Fulfillment or Inventory check activity which may fail and throw an exception. In a lot of the Payment integrations, the funds have already been `AUTHORIZE` or `AUTHORIZE_AND_CAPTURE` with respect to the Payment Gateway, and the Broadleaf system is now just handling the response back from the provider. If for some reason an exception is thrown, we need some way to perform a `VOID`, `REVERSE_AUTHORIZE`, or `REFUND` transaction based on the gateway's implementation to compensate for this exception during checkout.

The `PaymentGatewayRollbackService` interface contain several rollback methods that will get invoked by the `ConfirmPaymentsRollbackHandler` on any `AUTHORIZE` or `AUTHORIZE_AND_CAPTURE` that have been registered with it.

## Customizing the Rollback Logic

In most cases the Gateway module implementation will either issue a `VOID` or `REFUND` compensating request to the provider. If you don't want this to happen, you can easily override the default module implementation and inject your own logic, for example write to an error log which will get batched later.



