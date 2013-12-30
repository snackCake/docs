# Creating a Payment Gateway Module

Before you begin, it is recommended that you fully understand how the gateway works before attempting to implement
any of the BLC interfaces. Although all gateways have their own mechanisms of accepting and passing information back and forth, they all tend to follow similar patterns. The key is figuring out which pattern it falls into, making it easier to determine which of the Broadleaf [[Payment Gateway Interfaces]] should be implemented.

One way to help determine which communication mechanisms the gateways implement, is to take note as you read through
the Gateways documentation and answer the following questions:

## 1. How do you send Credit Card/Order Information to the Gateway?

- Is this a Hosted Solution? Will the customer see a button on the Cart or Checkout screen which when clicked will be
redirected out of your site to a Hosted Payment Page where payment/billing information is collected?
- Is this a Silent Post/Transparent Redirect Solution where Credit Card information is collected on a form managed on
your end which will then be POST'ed directly to the Payment Gateway for processing?
- Does the Gateway support both solutions? If so, you may want to support both in your implementation.

## 2. How does the Gateway communicate Transaction Information back to you?

Here are some examples if POST'ing Credit Card information directly to the Gateway:

- they may send back a 302 Redirect Response to the browser
with some information about the transaction (e.g. a token to get details of what happened)
- they may send back a 200 Response to a URL you specify 
with a hidden form that contains the result of the transaction. This form will then automatically submit itself using JavaScript.
- they may POST the transaction information directly to a publicly available URL on your system which will wait for a response back from you to relay back to the customer's browser.

## 3. How does the Gateway deal with errors?

Due to the vast differences in how gateways are implemented, errors/failures can happen in a variety of places within the payment process. It's important to understand where these errors can happen and how you and the gateway will deal with those errors in the event that one occurs.

For example: 

If the communication pattern is to POST the transaction results back to a publicly available URL, the Gateway may wait on a 200 response back from you to deem it a successful transaction. If there was an error completing checkout on your end, you can send back a 500 error and the gateway may be configured to automatically VOID the charged card. Again, this form of error handling is SPECIFIC to one particular gateway and may be completely different for other gateways. In many
cases, the Payment Gateway will charge the card and send you back a response. If for some reason an error occurs processing that response and completing checkout, it is up to you to manually VOID that payment if necessary. Therefore, you may need to implement the Rollback interfaces in order to handle these error flows.


## 4. How does the Gateway allow me to pass additional information not explicitly defined in their API.

In some cases, we may find it necessary to pass some additional information that we would need passed back to us in the repsonse. 

For example, one gateway provides an API to send extra custom fields that are pass through fields. In some of the modules built by Broadleaf, these fields may be used to hold identifying information about the Broadleaf Order. Many gateways allow this and it is important to keep in mind as you begin developing your solution in case you need it.


## Begin Development

Once you have the answers to these questions laid out and understood, you can start seeing where all this fits and where all the logic should go in terms of interfaces provided by Broadleaf.
