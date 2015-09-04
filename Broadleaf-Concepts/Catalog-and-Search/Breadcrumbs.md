## Breadcrumbs 

# Overview

Broadleaf provides a framework for building and displaying breadcrumbs.  The exact needs for handling breadcrumbs varies by implementation.   The components provided out of box have clear extension points for managing breadcrumbs.

# Key Components
|Component Name|Bean Id|Description|
|--------------|-------|-----------|
|BreadcrumbDTO| n/a | Domain object containing the link, type, and text|
|BreadcrumbType| n/a | An extendable Broadleaf enumeration indicating the type of breadcrumb HOME, SEARCH, CATEGORY, PRODUCT|
|BreadcrumbProcessor|blBreadcrumbProcessor|A Thymeleaf processor (blc:breadcrumbs) that calls the BreadcrumbService to generate the breadcrumbs and stores the result in a page variable named breadcrumbs|
|BreadcrumbService|blBreadcrumbService|The component responsible for generating the breadcrumbs.   It delegates to a series of handlers to generate each crumb|
|HomePageBreadcrumbServiceExtensionHandler|blHomePageBreadcrumbServiceExtensionHandler|Builds a single crumb whose name is "Home" or the value from the property breadcrumb.homepageText and whose link is the root (e.g. \)|
|SimpleSearchBreadcrumbServiceExtensionHandler|blSimplSearchBreadcrumbServiceExtensionHandler|Builds a single crumb with a search parameter to allow easy return to the last search|
|CategoryBreadcrumbServiceExtensionHandler|blCategoryBreadcrumbServiceExtensionHandler|Builds the breadcrumbs for the current category tree|
|ProductBreadcrumbServiceExtensionHandler|blProductBreadcrumbServiceExtensionHandler|Builds the product breadcrumb|


# Usage
Here is a sample snippet that can be added to the heat clinic to show breadcrumbs (below the nav element in `fullPageLayout`)

```html
<blc:breadcrumbs />

<div th:if="${breadcrumbs}" style="display: block">
     <ul class="breadcrumb">
        <li th:each="breadcrumb, iterStatus : ${breadcrumbs}">
            <a th:href="@{${breadcrumb.link}}" 
               th:text="${breadcrumb.type == 'SEARCH'} ? 'Search (' + ${breadcrumb.text} +')' : ${breadcrumb.text}"
               th:unless="${iterStatus.last}"></a> 
            <span th:text="${breadcrumb.type == 'SEARCH'} ? 'Search (' + ${breadcrumb.text} +')' : ${breadcrumb.text}"
               th:if="${iterStatus.last}"></span></li>
     </ul>
</div>
```

> Note that this specific HTML attempts to show the SEARCH term as a Breadcrumb and provides a link for all crumbs except for the last one 


# Relative URLs
It is common for implementations to build breadcrumbs relative to the users journey.    As an example, consider a product named "green-ghost" that is in the category "hot-sauces".

The "breadcrumb" for this product might normally be ...

Home > Hot Sauces > Green Ghost

If this product was also in the category "Clearance", some implementations may want to change the breadcrumb to represent the way the user accessed the product sometimes resulting in ...

Home > Clearance > Green Ghost

To aid with this type of requirement, the following approach can be used ...

**Step One** 
Enable product resolution by id by setting the `allowProductResolutionUsingIdParam=true` in common.properties.
> This will allow the ProductHandlerMapping to locate the product by id in addition to using the URL.


**Step Two** 
Use the Thymeleaf relative URL expression to build a relative url.  

For example, in productListItem.html, replace `*{url}` with `#blc.relativeURL(product)`
> This will take the last fragment of the product url and append it to the currentUrl and append the id of the product, resulting in a URL like `/clearance/green-ghost?productId=7` instead of using the default url e.g. `/hot-sauces/green-ghost`



