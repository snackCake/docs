# Checkout Page Design Considerations

## Flow Design

Out of the box, we aimed to provide a flexible checkout UX that works with most Payment Gateways. Of course, you can design your own checkout page, however depending on the Payment Gateway you integrate with will determine how your checkout page design is best laid out and may lead to some technical compromise.

For example, some gateways require that you pass in a billing address to be tokenized BEFORE you can create a secure token credit card form as the last step of your checkout process. You can accomplish this with Javascript, however does your site need to support Javascript disabled? These are some of the technical compromises that you and your business need to determine when designing your custom flow. Other gateways allow you to POST the billing address along with the credit card information directly to the gateway. Some gateways allow you to do both. In most cases we've seen a lot of gateways allow you to pass in the billing address first. With that in mind, we designed the checkout page to collect a billing address as one of the first steps in the process. Here is a wireframe of what our out-of-the-box checkout page looks like:

![Checkout Page Design](checkout-page-design.png)

A few things to note. First if you are not logged in, we will present the user with a login option or the ability to checkout as guest. Next, if this order requires a billing address, we'll go ahead and ask the user to fill out their billing address first. Next, they will have the ability to fill out their shipping address (or choose same as billing) and choose their delivery method. Note, that whenever they select a new delivery option, the price of their order may change. Finally, they will have the ability to add any other Payment methods (gift cards/customer credit) and enter their credit card information. Once they hit the final submit, their Credit Card information is POST'ed directly to the Payment Gateway for processing along with any other tokenized order information that is specific for that gateway.

Another important consideration is that the different sections may or may not be relevant based on your current order state. For example, if your order contains non-shippable items (e.g. digital downloads) it does't make sense to ask for shipping information. Or, if you have applied a payment method like (PayPal Express Checkout), it doesn't make sense to ask for a billing address.

We provied a Thymeleaf `OnePageCheckoutProcessor` to help render and make those decisions on when to draw the different states that are required for the checkout page.

## Thymeleaf Credit Card Form

We've aimed to make the integrations with Payment Gateways seamless and transparent. We've also aimed to lessen any PCI-compliance burdens when dealing with Credit Card information. With that in mind, most of the integrations with the Payment Gateways that we provide offer a "Transparent Redirect" or "Direct Post" form of transaction communication (both terms are synonymous) where Credit Card information is sent directly to the Gateway bypassing your web servers.

We provide a Thymeleaf processor that renders the Credit Card form based on the payment integration you choose. It will change the POST action to the correct endpoint as well as add any hidden fields to the form that your particular payment integration may need in a transaction request.

## Handling Alternative Checkout Flows and Considerations

Some payment gateways like "PayPal Express Checkout" offer alternative checkout flows that differ from a regular one page checkout. PayPal Express Checkout requires that you place the PayPal button on your Cart Page as well as offer it as a form of payment as the last step of your process. 

When you place the PayPal Express Checkout button on your cart page. The user will be redirected to PayPal's site to enter their details. After completing this they will then be redirected back to your site as the order has not yet been completed. They have not yet entered their Shipping Address or picked their Delivery Method.

Our `OnePageCheckoutProcessor` will handle this particular order state where there is an OrderPayment on the order that is what we called `UNCONFIRMED`. Since there is an Order Payment that is unconfirmed from a `Third Party Account`, our checkout page won't bother asking for a billing address and will only allow you to complete checkout using that payment that's in process.

So, if you are integrating with a Payment Gateway that has alternative checkout flows, be mindful of the different payment states that can occur and how your UI will best handle it.
