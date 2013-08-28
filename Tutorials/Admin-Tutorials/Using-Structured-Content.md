#Using Structured Content

Structured content allows you to manage content via the admin which can be displayed on your site without the need to redeploy. By doing so you can enter in new promotions, ads, events or anything you want to show up.

There are two steps to getting Structured Content setup, the first is setting up the html to display the content and the second is entering the content via the admin.

##HTML

In your HTML you will call the `<blc:content />` tag and pass in the parameter of the content type that you want to access. It will return a `contentItem` which can then be used to access the field values of the content. We can see how this is done from the following snippet pulled from the Broadleaf DemoSite.

```html
<blc:content contentType="Homepage Banner Ad" />
<div id="banners" th:if="${contentItem}">
    <a th:href="@{${contentItem['targetUrl']}}"><img th:src="@{${contentItem['imageUrl']}}" alt="Buy One Get One" /></a>
</div>
```

The above snippet will display content of type **Homepage Banner Ad** with the highest priority found. The priority that is set will dictate if it will be chosen above others. If you choose to retrieve to pieces of content, and you have setup 3 pieces with priorities 1, 2 and 3 respectively, only the ones with 1 and 2 will be retrieved. If the priority is the same for one or more pieces of content, then they will be chosen/displayed randomly.

###The `<blc:content />` tag has several parameters it can take which it allows you to fine tune what you need to display.

We will cover some of the more commonly used ones but a full list of parameters can be found in the [Java Documentation](http://javadoc.broadleafcommerce.org/2.0.1/contentmanagement-module/org/broadleafcommerce/cms/web/processor/ContentProcessor.html)

1. contentType (required) - specifies the content you are retrieving

2. maxResults - if specified limits the results to a specified number of items. The content will be returned according to priority. If content items share the same priority, then they will be returned randomly.

3. contentListVar - allows you to specify an alternate name for the list of content results. By default, the results are returned in the page attributed `contentList`.

4. contentItemVar - since a typical usage is to only return one item, the first item is returned in the variable `contentItem`. This variable can be used to change the attribute name.


##Admin

1. In the admin, go to **Content Management** and go to **Structured Content**.

2. Select **Add Structured Content**. You will see the following list of Structured Content Types. Select **Homepage Banner Ad**, which is what we are displaying in the code snippet above.

![Structured Content Types](using-sc-tutorial-1.png)

3. Enter in the relevant information and save.

That's all you need. Now if you want to introduce a new Homepage Banner Ad on your site, you can add it through the admin. You can also set rules, so you can, for instance, show one add to an anonymous customer, but once they log in, you can show them one tailored to your registered customers.

Creating your own Structured Content Type and setting up rules to control when to display the content will be covered in different tutorials (coming soon).