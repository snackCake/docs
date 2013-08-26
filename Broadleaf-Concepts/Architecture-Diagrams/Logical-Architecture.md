# Overview
Broadleaf Commerce is a flexible framework that can play a variety of roles within an enterprise commerce system.    The logical architecture described below represents a Broadleaf Commerce application as the primary engine in an enterprise eCommerce application.     It depicts a layered physical architecture consisting of a web layer, application layer, search layer, and data layer.

## Web Layer
The Web Layer receives requests from the public internet. 
![BLC Web Layer](architecture/BroadleafCommerceLogicalArchitectureWebLayer.png)
###Web Layer Component Descriptions
| Component                          |  Description                                             |
|:-----------------------------------|:---------------------------------------------------------|
|Load Balancer _(optional)_ | Incoming web requests will typically be received by a load balancer which is configured to direct the requests to one or more web servers.|
|Web Server(s) | It is recommended that Broadleaf Commerce applications utilize an Apache Web Server with the modjk plugin with session affinity to load balance requests to the application servers|
|CDN _(optional)_|Most sites will benefit from a using a 3rd party CDN (Content Delivery Network) such as Akamaii or Amazon Cloud Front.   Using a CDN along with Broadleaf Commerce requires a small amount of configuration and offers your customers the benefit of faster load image, video, and content load times. |

## Application Layer
The Application Layer is where the Broadleaf Commerce framework code executes.   This layer handles requests that have been forwarded from the Web Layer.    The diagram below shows a configuration where the application layer is physically separated for admin, site, and api requests.    It is typical to combine these application layer components into a single set of web servers.    We illustrate the fact that the components can be separated for architectures that need this this level of flexibility.
![BLC App Layer](architecture/BroadleafCommerceLogicalArchitectureApplicationLayer.png)
###Application Layer Component Descriptions
| Component                          |  Description                                             |
|:-----------------------------------|:---------------------------------------------------------|
|Application Server | Broadleaf Commerce can be used with any application server that supports the version of Spring and Hibernate being used by the version you wish to deploy.    We strongly recommend Tomcat as the application server but users have successfully run the system on Weblogic, Wepshere, and JBoss|
|Admin Server | Broadleaf Commerce provides an administration server that can be used to manage catalogs, products, offers, targeted content, and static pages amoung other things.    The admin can be run in the same application server as the site or distributed separately as required by your application.   This decision can be based on many facets but is typically based around exposing the admin only to users with access to an internal network _(versus the public internet)_.    Some architect's may desire the flexibility to scale the site, api, and admin traffic separately; however, it is not generally required.|
|API Server| Broadleaf Commerce can be configured to serve as an API server for the framework functionality.   Sites may choose to expose the API through their public website (e.g. application server) or as a separate instance.   Both configurations are easily achieved.|

## Search Layer
Broadleaf Commerce by default uses an embedded search layer.   A separate physical Search Layer is recommended for catalogs with over 10,000 unique priceable items.   When calculating your catalog size, you should multiply by the number of price-lists (and/or) internationalized products.   For example, a site that has 5,000 products but also 3 distinct price-lists (e.g. different currencies for each of those items) should use a separate physical search layer since the number of priceable items in this case is (5,000 products times 3 currencies) is 15,000.
![BLC Search Layer](architecture/BroadleafCommerceLogicalArchitectureSearchLayer.png)
###Search Layer Component Descriptions
| Component                          |  Description                                             |
|:-----------------------------------|:---------------------------------------------------------|
|SOLR | SOLR is a highly scalable search engine that can run in a variety of configurations.   You should consult the SOLR documentation for instructions on setting up a cluster. |

## Database Layer
Broadleaf Commerce is Database agnostic and works with any database that is compatible with Java, JPA, and Hibernate.    This includes popular commercial database platforms like Oracle and MS-SQL Server as well as open source database platforms like MySQL and PostGRES.
![BLC DB Layer](architecture/BroadleafCommerceLogicalArchitectureDBLayer.png)
###DB Layer Component Descriptions
| Component                          |  Description                                             |
|:-----------------------------------|:---------------------------------------------------------|
|Database | For most sites, MySQL performs sufficiently.    Choosing a DB platform for an enterprise eCommerce system is an important decision with a number of factors.   Each application exercises the DB in unique ways and for even moderate sites (~1,000 orders a day) some tuning is typically required.   For larger sites the most important criteria (we believe) is to choose a DB platform that your organization has the capability to support. |
