# Understanding the Transaction Lifecycle

It's important to understand the lifecycle of a Payment Transaction and how its state can affect which operations can be performed on it.

## Authorize or Authorize and Capture
In all cases, the initial transaction state will either be an `Authorize` or an `Authorize and Capture`. This should be configurable by the interface method `PaymentGatewayConfigurationService.isPerformAuthorizeAndCapture()`. If this is true, then the system will initiate a payment transaction using `PaymentTransactionType.AUTHORIZE_AND_CAPTURE`, otherwise it will use `PaymentTransactionType.AUTHORIZE`

## Reverse Authorize, Capture, Void, and Refund
So, once a payment has initially been sent to the processor, it's state will either be Authorized or Authorized and Captured (Some gateways use the term `Sale` or another term to represent this). From this point, if a transaction is just Authorized, you can either Reverse Authorize the transaction or Capture it. 

Once the transaction has been captured, (either from a Capture request or from initially creating an Authorize and Capture request), there is period where the money has not left the Card Issuer's Bank into the Merchant's Account. This is known as being submitted for settlement. During the time, and only this time can a transaction be Voided. Once the funds have been transferred, the transaction is considered Settled. 

Once a transaction has been settled, then and only then will you have the ability to Refund the transaction. 

It's important to note that some Gateways may have slightly different terminology, but the concepts are the same. Also, some Gateway SDK's may combine the Reverse Authorize and Void methods into just Void, as the processor will know what to do based on it's current state. Please document and read your gateway's implementation for details.
