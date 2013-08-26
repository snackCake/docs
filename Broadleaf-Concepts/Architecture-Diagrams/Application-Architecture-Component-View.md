Broadleaf Commerce is highly modular, highly extensible framework for building commerce applications.   The system leverages other open source frameworks to provide a best of breed development platform to meet the challenges of enterprise software development.   

##Key Related Components

|Related Component Name | Description|
|--------------------|-----------------------------------------|
|Spring Framework|[Spring](http://www.springsource.org) provides the component architecture used by Broadleaf Commerce.  |
|Spring Security|Spring Security is leveraged by Broadleaf Commerce for authentication and authorization.|
|Spring MVC|Spring MVC is the recommended view layer technology for a Broadleaf Commerce application.|
|Hibernate|[Hiberante](http://www.hibernate.org) is used for all persistence operations in Broadleaf|
|Thymeleaf|[Thymeleaf](http://www.thymeleaf.org) is the recommended templating engine and is recommended instead of JSP for Broadleaf|
|SOLR|[SOLR](http://lucene.apache.org/solr/) is the search engine technology used by Broadleaf Commerce.

##About Broadleaf's Modular Architecture
Broadleaf Commerce organizes the project into modules.   The modules can be categorized as core framework, add-on, or third party modules.   

###The core framework
The core framework consists of the modules in the following table.   It would be exceptionally rare for a Broadleaf Commerce implementation NOT to use all of these core modules.

|Core Module Name            | Description                                                              |
|----------------------------|--------------------------------------------------------------------------|
|Framework|This generically named module represents the commerce functionality of Broadleaf Commerce (e.g. Orders, Products, Offers, etc.|
|Profile|This module provides the concept of Customer.  It is provided separately from framework in anticipation that some may want to utilize these features without using the Commerce features.|
|CMS|This module provides content management functionality that supports targeting ad based content to users based on their profile as well as static page management.|
|Open Admin|The Broadleaf Commerce admin architecture which allows annotated JPA entities to be administered via a rich UI.   The intent of separating this module is to provide some architecture purity while leaving open the possibility that the admin techniques used by Broadleaf Commerce may be provided outside of the Commerce application in the future.|

###Third Party Add On Modules
Third party add-on modules involve an integration with Broadleaf Commerce and another system.   Typical uses of these include integrations with payment providers like PayPal, Braintree, and CyberSource.

###Third Party Modules
Third party add-on modules involve an integration with Broadleaf Commerce and another system.   Typical uses of these include integrations with payment providers like PayPal, Braintree, and CyberSource.

###Add-on Modules
Add on modules represent functionality that can be incrementally added to the Broadleaf Commerce framework.     Add-on modules may be free, open source or commercial.  

Examples of free, open source modules include the Inventory and SEO modules.  

Examples of commercial modules include Account Credits, MultiTenant, and Workflows.
