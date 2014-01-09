# Creating a Payment Gateway Module

Before you begin, it is recommended that you fully understand how gateways works before attempting to implement
any of the BLC interfaces. Please make sure to read [[Understanding Payment Gateways]] first to aid in this process.

## Conventions

We'll go over some of the conventions to create consistency across all Payment Modules.

### 1. Here is the package structure that many of these Broadleaf Payment Modules follow:

All implementations of the BLC `Common` interfaces will be in the following packages

- `org.broadleafcommerce.payment.service.gateway`
- `com.broadleafcommerce.payment.service.gateway`

All gateway specific services and constants will go under the following package structure

- `org.broadleafcommerce.vendor.replace_with_gateway_name.service`
- `com.broadleafcommerce.vendor.replace_with_gateway_name.service`

All gateway specific web components such as Spring MVC Controllers and Thymeleaf Processors will go under

- `org.broadleafcommerce.vendor.replace_with_gateway_name.web`
- `com.broadleafcommerce.vendor.replace_with_gateway_name.web`

### 2. Utilize any SDK that the gateway may provide.

Note that some gateways don't publish their Java SDK on Maven Central, so you may need to install it locally and in your documentation note that their is a dependency on their JAR for compilation. If this is the case, please make note of that and where to find that dependency in your documentation. 

### 3. Utilize `AbstractExternalPaymentGatewayCall`

In a lot of cases, you may notice that a lot of the transaction methods share common code to create a Request which then calls an external API or SDK and parses the Response. If this is the case, you may wish to create a parent class that all these extend to unify this boiler plate code. It's also important to extend `AbstractExternalPaymentGatewayCall` if your service makes an SDK or External API call. This allows anyone using the framework to configure the ServiceMonitor AOP hooks and detect any outages to provide (email/logging) feedback when necessary. You can look at the Java Docs on that class for further examples and documentation.

### 4. Creating endpoints within the Modules

Each of the Payment Modules will now provide out-of-the-box endpoints to handle processing web responses back from the Gateway. These Spring MVC controllers will reside under the `.../vendor/replace_with_gateway_name/web/` package. These controllers will provide default @RequestMappings and extend `PaymentGatewayAbstractController`

### 5. Make sure to capture the Raw Response on your PaymentResponseDTO.

It's often very helpful and in some cases required to capture the entire response back from a gateway. Make sure to serialize all the information coming back from the gateway in the Raw Response field. You may wish to use the `PaymentGatewayWebResponsePrintServiceImpl` to translate an `HttpServletRequest` into a Raw Response String.

## Implementation Guidelines

Here are some steps to follow to help get you started developing your own module.

1. Extend and Implement the `PaymentGatewayConfiguration`. Every module should provide a configuration bean that provides information about what it can and cannot handle as well as any specific configuration parameters it needs.

2. Implement the `PaymentGatewayConfigurationService`. Every module should provide an implementation of this service that will define all the Payment Gateway Interfaces that this module supports.

3. Implement the `PaymentGatewayTransactionService` to handle any post Authorize or Authroize and Capture operations.

4. Implement the `PaymentGatewayWebResponseService`. In most cases, the Gateway will send back the transaction information back to your system using an `HTTPServletRequest`. Use this interface method to encapsulate translating this into a `PaymentResponseDTO`.

5. Implement a Spring MVC controller that receives the results of the transactions that extends `PaymentGatewayAbstractController`. This abstract controller expects certain methods to be implmented by the Gateway specific implementation.

6. Implement the `PaymentGatewayRollbackService` to handle error scenarios. This API will be called in the event an error gets thrown in the Checkout Workflow after a payment has been confirmed. That means the payment has already been captured or authorized however something else in the workflow has caused an exception to occur which therefore needs to rollback any payments that have already been processed. 

## Hosted or Transparent Redirect?

If it is a Hosted Solution:

- Implement the `PaymentGatewayHostedService`
- Create a Thymeleaf Processor to render the button/form needed to redirect to the hosted page.

If it is a Transparent Redirect/Silent Post Solution:

- Implement the `PaymentGatewayTransparentRedirectService`
- Create a Thymeleaf Processor Extension Handler that extends `AbstractTRCreditCardExtensionHandler`. This will dynamically change a Credit Card Form (see `TransparentRedirectCreditCardFormProcessor`) on the checkout page and changes its ACTION URL and append any gateway specific hidden fields to that form.
- Create a Thymeleaf Expression Extension Handler that extends `AbstractPaymentGatewayFieldExtensionHandler`. This will dynamically change any HTML field `name` attributes to be those that are expected from the gateway.
 
## Example Gateway

The Heat Clinic Demo Site includes a test "Null Payment Gateway" implementation that shows the implementations involved to integrate an actual payment gateway. This is not an actual payment gateway and in no way should be used in production. This was created for demo purposes only and is helpful in showing how all the payment "plumbing" is configured.

View the classes in the package `DemoSite/core/src/main/java/com/mycompany/sample`


## Testing

You can mock the Payment Request DTO without worrying about creating an actual Broadleaf Order since modules are only dependent on BLC `Common`. This allows you to create isolated integration tests for each service.

If you've followed the package structure outlined above, it is helpful to enable trace debugging for the following:

```xml    
    <logger name="org.broadleafcommerce.vendor" level="TRACE"/>
    <logger name="com.broadleafcommerce.vendor" level="TRACE"/>
    <logger name="org.broadleafcommerce.common.web.payment" level="TRACE"/>
```    



