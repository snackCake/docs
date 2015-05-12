# Add-On Modules

Broadleaf Commerce can be enhanced with add-on modules.    

## Module Licenses
| License Type | Description | How to get it? |
| :----------- | :---------- | :------------ |
| Community | Indicates the module uses the open source Apache 2 license | [Maven Central](http://search.maven.org/) |
| Enterprise | A commercial offering from Broadleaf Commerce, LLC | ([contact us](http://www.broadleafcommerce.com/contact))|

## Module Categories

| Module Category | Description |
| :----------- | :---------- |
| [Enterprise Modules](#enterprise-modules) | Provide enterprise feature add-ons to Broadleaf |
| [Payment Modules](#payment-modules) | Provide payment processing. |
| [Wallet Modules](#wallet-modules) | Provide digital wallet solutions for easy checkout. |
| [Tax Modules](#tax-modules) | Provide tax calculations. |
| [Shipping Modules](#shipping-modules) | Provide shipping options and calculations. |

## <a name="enterprise-modules"></a>Enterprise Modules 
### Workflows and Approvals
* Ability to modify products, offers, prices in a sandbox
* Preview changes before they go live
* Schedule when changes go live
* [Full Module Documentation](http://docs.broadleafcommerce.org/enterprise/current/module-installation)

### Theme Module
* Provides the ability to use configurable themes 
* Easily manage theme specific variables in the Broadleaf Admin
* Theme variables can be used to control CSS, JavaScript, and Functional aspects of the implementation
* [Full Module Documentation] (http://docs.broadleafcommerce.org/theme/current)

### Multi-Tenant Module
* Flexible, extensible multi-tenant (multi-site) solution
* Shared or separate customers and orders
* Support for separate, shared, or partially shared catalogs per site
* [Full Module Documentation] (http://docs.broadleafcommerce.org/multitenant/current)

### Advanced Pricing (PriceList) Module 
* Manage separate price lists
* Flexible rule based price list determination based on customer, locale, time
* [Full Module Documentation] (http://docs.broadleafcommerce.org/pricelist/current)

### Advanced Offers Module
* Tiered Offers (Allows a single offer that varies the discount based on quantity purchased)
* Customer Specific TimeZone Offers (Ability to run a special from 5-6pm in the customer's timezone or a specific timezone)
* [Full Module Documentation] (http://docs.broadleafcommerce.org/advancedoffer/current)

### Custom Fields
* Add new fields to product, customer from the Broadleaf admin
* Fields are automatically available for use in offer and content targeting rules
* [Full Module Documentation] (http://docs.broadleafcommerce.org/customfield/current)

### Advanced CMS
* Extends the Broadleaf Structured Content concept with more intuitive admin organization
* Supports templates with tiles that can serve mixed content

### Subscription
* Adds subscription fields to the order and product domain
* Supports subscription offers such as get 50% off for the first three months
* [Full Module Documentation] (http://docs.broadleafcommerce.org/subscription/current)

### OMS
* Creates Fulfillment Orders after an order has been submitted
* Support for separate shipments, item cancellation, capture payment on fulfillment, and refunds

### Advanced Inventory
* Manage inventory at different Fulfillment Locations
* Allow customers to subscribe to be notified when inventory is available for out-of-stock Skus
* Deal with high-contention inventory scenarios
* [Full Module Documentation](http://docs.broadleafcommerce.org/advancedinventory/current)

### Content Tests
* Allows creation of content tests in the admin using Google Experiments
* Provides A/B testing functionality based on a test URL and multiple variations.
* [Full Module Documentation] (http://docs.broadleafcommerce.org/contentTests/current)

### Gift Card and Account Credit
* Allows customers to purchase and pay for items with a gift card
* Allows a CSR (or by some other means) add store credit for a customer
* [Full Module Documentation](http://docs.broadleafcommerce.org/giftcardandcustomercredit/current)

### Scheduled Jobs and Events
* Allow for jobs to be taken and events to be dispatched across multiple application servers
* [Full Module Documentation](http://docs.broadleafcommerce.org/scheduledjobs/current)

## <a name="payment-modules"></a>Payment Modules

| Module Name | License | Description |
| :---------- | :------ | :---------- |
| [Authorize.net] (http://docs.broadleafcommerce.org/authorizenet/current) | Enterprise | Payments through CyberSource's [Authorize.net](http://www.authorize.net) gateway |
| [Paypal] (http://docs.broadleafcommerce.org/paypal/current)  | Enterprise | Payments through PayPal's [express checkout](https://www.paypal.com/webapps/mpp/express-checkout) |
| [PayPal Payflow Pro] (http://docs.broadleafcommerce.org/paypal-payflowpro/current) | Enterprise | Payments through PayPal's [Payflow Pro](https://www.paypal.com/us/webapps/mpp/referral/paypal-payflow-pro) |
| [Braintree] (http://docs.broadleafcommerce.org/braintree/current) | Enterprise | Payments through [Braintree Payments](https://www.braintreepayments.com/) |
| [CyberSource Payment] (http://docs.broadleafcommerce.org/cybersource-payment/current) | Enterprise | Payments through CyberSource's [Secure Acceptance Silent Order Post](http://www.cybersource.com/resources/collateral/pdf/SecureAcceptance_SilentOrderPost.pdf) and CyberSource's [SOAP API](http://www.cybersource.com/developers/learn/integration_methods/simple_order_api/learn_more.php) |
| [Sagepay] (http://docs.broadleafcommerce.org/sagepay/current)  | Enterprise | Payments through [Sagepay](http://www.sagepay.co.uk/) |

## <a name="wallet-modules"></a>Wallet Modules
| Module Name | License | Description |
| :---------- | :------ | :---------- |
| [MasterPass] (http://docs.broadleafcommerce.org/masterpass/current)  | Enterprise | Digital wallet solution through [MasterPass by MasterCard](https://masterpass.com/) |

## <a name="tax-modules"></a>Tax Modules

| Module Name | License | Description |
| :---------- | :------ | :---------- |
| [Avalara] (http://docs.broadleafcommerce.org/avalara/current)  | Enterprise | Sales tax through [Avalara] (http://www.avalara.com/) |
| [TaxCloud] (http://docs.broadleafcommerce.org/taxcloud/current)  | Enterprise | Sales tax through [TaxCloud] (http://www.taxcloud.net) |

## <a name="shipping-modules"></a>Shipping Modules

| Module Name | License | Description |
| :---------- | :------ | :---------- |
| FedEx | Enterprise | Shipping quotes from [FedEx web services](http://www.fedex.com/us/developer/product/basics.html) |
| [USPS] (http://docs.broadleafcommerce.org/usps/current)  | Enterprise | Shipping quotes from [USPS web tools](https://www.usps.com/business/web-tools-apis/welcome.htm) |


## Community Modules
### Amazon Module
* Provides an S3 implementation of the FileServiceProvider interface
* Allows the images loaded via the Broadleaf admin to go directly to amazon S3
* Allows generated files like sitemap.xml to be stored on S3
* [Full Module Documentation] (http://docs.broadleafcommerce.org/amazon/current)

### Rackspace Modules
* Provides a Cloud Files implementation of the FileServiceProvider interface. This allows uploaded admin images and generated site maps to be stored on Cloud Files rather than the local file system
* Compatible with the automatic Cloud Files CDN through Akamai
* [Full Module Documentation](http://docs.broadleafcommerce.org/rackspace/current)

