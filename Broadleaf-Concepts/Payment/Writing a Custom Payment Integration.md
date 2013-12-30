# Writing your own payment integration

While Broadleaf provides some modules for payment processing (see [[Modules]] for a list of the providers we have implemented with), it is likely that you need to write a processor to communicate with a different payment processor. This does not require an in-depth knowledge of the Broadleaf domain but rather some knowledge of common DTOs that we have provided to facilitate the different types of communication with gateways.

Decide which interfaces that you are going to support

List of all of the interfaces that Broadleaf has and what do they do

## 1.

- Spring Controllers
	- Receiving a response from the gateway
- Confirming transactions (providing a review page)
- Hooking into the checkout screen with Thymeleaf processors
	- Extension handlers for swapping out predefined form fields
	- Extra hidden fields for the payment processor
- Hooking up to the test harness (probably an internal doc)