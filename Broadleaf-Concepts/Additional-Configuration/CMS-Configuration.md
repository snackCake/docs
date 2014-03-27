# CMS Configuration

The Broadleaf Content Management system was designed to support the needs of typical enterprise eCommerce implementations.

- **Static Page Management** - Provides users to create a static managed page.
- **Structured Content Management** - Provides users with a way to manage "Ads" and other structured content
- **Asset Management** - Provides a way for end users to load new assets into the Broadleaf system
- **Workflow** - The CMS is designed with a multi-step workflow with preview and approval capability.

## Configuration

- **Page Templates** - The page templates that users are able to choose from in the BLC admin are defined in the database. This definition includes the fields that the user will be able to enter and the associated underlying JSP template that will render the page
- **Structured Content Types** - Structured Content can be used in a number of ways. A typical usage will be to provide business user's with control of a portion of the page.   For example, in the Broadleaf Demo application, there are three types of structured content (The welcome message, the banner ad, and the two ads on the right hand side of the page). Each of these represents a content-type that was defined in the database.

## Data Model

### Structure of the CMS Data

Page templates and structured content for a Broadleaf instance are data driven. You can find detailed ERD diagrams of these tables at [[CMS Model]]

### Storage of the actual CMS Data

The following structures hold your ACTUAL instance data (e.g. actual pages, ads, etc.) that is managed by your business users. You should bring this data over if you the data in your import.sql is appropriate and should be part of the conversion.

- **Page Data** - BLC\_PAGE, BLC\_PAGE\_FLD, BLC\_PAGE\_FLD\_MAP
- **Structured Content Data** - BLC\_SC, BLC\_SC\_FLD, BLC\_SC\_FLD\_MAP

## Example Developer Usage

You can find examples of structured content usage in `layout/home.html`.   The following code shows the main banner ad.

```html
<blc:content contentType="Homepage Banner Ad" />       
<div id="banners">
    <a th:href="@{${contentItem['targetUrl']}}">
        <img th:src="@{${contentItem['imageUrl']}}" />
    </a>
</div>
```
