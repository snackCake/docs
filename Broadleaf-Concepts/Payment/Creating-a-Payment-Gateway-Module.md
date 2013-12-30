# Creating a Payment Gateway Module

Before you begin, it is recommended that you fully understand how the gateway works before attempting to implement
any of the BLC interfaces. Although all gateways have their own mechanisms of accepting and passing information back and forth,
they all tend to follow similar patterns. The key is figuring out which pattern it falls into, making it easier to determine
which of the Broadleaf [[Payment Gateway Interfaces]] should be implemented.

One way to help determine which communication mechanisms the gateways implement, is to take note as you read through
the Gateways documentation and answer the following questions:

## 1) How do you send Credit Card/Order Information to the Gateway?

- Is this a Hosted Solution? Will the customer see a button on the Cart or Checkout screen which when clicked will be
redirected out of your site to a Hosted Payment Page where payment/billing information is collected?
- Is this a Silent Post/Transparent Redirect Solution where Credit Card information is collected on a form managed on
your end which will then be POST'ed directly to the Payment Gateway for processing?
- Does the Gateway support both solutions? If so, you may want to support both in your implementation.

## 2) How Does the Gateway Communicate Transaction Information back to you?
Here are some examples if POST'ing Credit Card information directly to the Gateway:
- they may send back a 302 Redirect Response to the browser
with some information about the transaction (e.g. a token to get details of what happened)
- they may send back a 200 Response to a URL you specify 
with a hidden form that contains the result of the transaction. This form will then automatically submit itself using JavaScript.
- they may POST the transaction information directly to a publicly available URL on your system while waiting a response
back from you to relay back to the customer's browser.
