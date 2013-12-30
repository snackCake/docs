# SEO 

The Broadleaf Commerce framework provides a number of SEO related features including the following.
- SEO friendly URLs
- SiteMap generation
- Robots.txt maintenance
- URL Redirect maintenance
- SEO Meta-Data 
- Optimized CSS and JavaScript

## SEO Friendly URLs 
Custom Pages, Products, and Categories can all be maintained in the Broadleaf admin.   Each of these has a URL property that allows the admin to specify an SEO friendly URL.

The URLs are handled via custom URL handlers. Namely, ^[javadoc:org/broadleafcommerce/cms/web/PageHandlerMapping], ^[javadoc:org/broadleafcommerce/core/web/catalog/ProductHandlerMapping], and ^[javadoc:org/broadleafcommerce/core/web/catalog/CategoryHandlerMapping].    These classes all implement the Spring MVC - `AbstractHandlerMapping` allowing them to delegate matching URLs to the related controllers (for example `PageController`).

These handlers execute in the following order (Product, Page, Category). 

## SiteMap Generation
For many sites, the sitemap is an important part of the SEO strategy.  Broadleaf provides a configurable, extensible pattern for generating your sitemap.xml file.

The structure for sitemaps is a standard maintained at [sitemaps.org] (http://www.sitemaps.org/protocol.html).

A simple version from the sitemap.org website follows:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
   <url>
      <loc>http://www.example.com/</loc>
      <lastmod>2005-01-01</lastmod>
      <changefreq>monthly</changefreq>
      <priority>0.8</priority>
   </url>
</urlset> 
```

Notice the `url` element in the example above.   An eCommerce site could have a large number of these (1 for each product).    The sitemap specification indicates a maximum of 50,000 entries per file which results in the need for "indexed" sitemap files.   An example from sitemap.org follows ...
```xml
<?xml version="1.0" encoding="UTF-8"?>
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
   <sitemap>
      <loc>http://www.example.com/sitemap1.xml.gz</loc>
      <lastmod>2004-10-01T18:23:17+00:00</lastmod>
   </sitemap>
   <sitemap>
      <loc>http://www.example.com/sitemap2.xml.gz</loc>
      <lastmod>2005-01-01</lastmod>
   </sitemap>
</sitemapindex> 
```

Broadleaf provides components to generate the sitemap based on the products, categories, and pages that you have setup in your system.   Other url entries can be built by creating your own custom SiteMapGenerators following the same approach.

The default implementation automatically indexes the files if needed.  

The approach centers on ^[javadoc:org/broadleafcommerce/common/sitemap/service/SiteMapGenerator] components which are responsible for generating individual URL elements.

The key components involved are ...
|Component Name|Component Description|
|^[javadoc:org/broadleafcommerce/common/sitemap/service/SiteMapService]|The primary process that is responsible for building the sitemap.|
|^[javadoc:org/broadleafcommerce/common/sitemap/service/SiteMapBuilder]|Helper class responsible for much of the file processing.| 
|^[javadoc:org/broadleafcommerce/common/sitemap/service/SiteMapGenerator]|Rresponsible for building url entries for the sitemap| 
|^[javadoc:org/broadleafcommerce/common/sitemap/controller/BroadleafSiteMapController.java |A basic controller to render the SiteMap|

In addition, the system provides the following generators out of box ...
|Generator Name|Purpose|
|^[javadoc:org/broadleafcommerce/cms/page/service/PageSiteMapGenerator]|Builds the url entries for pages| 
|^[javadoc:org/broadleafcommerce/core/catalog/service/CategorySiteMapGenerator]|Builds the url entries for categories|
|^[javadoc:org/broadleafcommerce/core/catalog/service/ProductSiteMapGenerator|Builds the url entries for products| 
|^[javadoc:org/broadleafcommerce/common/sitemap/service/CustomUrlSiteMapGenerator]|Defines custom url entries| 

Each of these components can be overridden and additional for implementation specific behavior and more complex implementations are likely to need custom SiteMapGenerators. 

## Robots.txt maintenance
Broadleaf allows you to maintain your robots.txt file in the Broadleaf Admin.    Simply create a page named '/robots.txt' and it will be served when requested.

This is accomplished via the `BroadleafRobotsController`.  The Broadleaf Demo Site provides an example usage with the `RobotsController` implementation.

> Note that the name and location of the `sitemap.xml` file should match your `SiteMapController` and `SiteMapGeneratorConfiguration`.   The out of box defaults work fine for most implementations.

## URL Redirect maintenance
The Broadleaf admin provides the ability to create 301 redirects using the URL Redirect maintenance.  These URLs can be manged in the Content Management section of the admin.   
