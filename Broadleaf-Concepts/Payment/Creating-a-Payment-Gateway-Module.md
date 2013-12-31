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

## Implementation Guidelines



