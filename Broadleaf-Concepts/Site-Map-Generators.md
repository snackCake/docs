#Site Map Generators

The Broadleaf framework comes with 5 build-in site map generators:  Custom(manually enter URLs into the database), Page, Product, Category, and Advanced Structured Content.  Each site map generator is set to create site map files for all URLs in a given category.  Should more than one site map file need to be created (the default limit is set to 50,000 URLs per file, but the limit is configurable) a site map index file is also created.

Additional site map generators can also be created by adding 4 new files:

1)  A class that extends `SiteMapGeneratorType`.  This class will allow you to add your own site map generator type.  

Here is an example:

```java
public class AdvancedStructuredContentSiteMapGeneratorType extends SiteMapGeneratorType {

    public static final SiteMapGeneratorType STRUCTURED_CONTENT = new SiteMapGeneratorType("STRUCTURED_CONTENT", "Structured Content");

}
```
2)  An interface that extends `SiteMapGeneratorConfiguration`.  This will allow you to add your own site map generator configurations.  Each combination of configurations is used to specify the priority and change frequency of a group of site map URLs.

Here is an example:

```java
public interface CategorySiteMapGeneratorConfiguration extends SiteMapGeneratorConfiguration {

    /**
     * Returns the root category.
     * 
     * @return
     */
    public Category getRootCategory();

    /**
     * Sets the root category.
     * 
     * @param rootCategory
     */
    public void setRootCategory(Category rootCategory);
```

3)  A class that extends `SiteMapGeneratorConfigurationImpl` and implements your interface from above.

Here is an example:
```java
@Entity
@Table(name = "BLC_CAT_SITE_MAP_GEN_CFG")
@AdminPresentationClass(friendlyName = "CategorySiteMapGeneratorConfigurationImpl")
public class CategorySiteMapGeneratorConfigurationImpl extends SiteMapGeneratorConfigurationImpl implements CategorySiteMapGeneratorConfiguration {

    private static final long serialVersionUID = 1L;

    @ManyToOne(targetEntity = CategoryImpl.class)
    @JoinColumn(name = "ROOT_CATEGORY_ID", nullable = false)
    @AdminPresentation(friendlyName = "CategorySiteMapGeneratorConfigurationImpl_Root_Category")
    @AdminPresentationToOneLookup()
    protected Category rootCategory;
```

4)  Most importantly a class that extends `SiteMapGenerator`.  This will be where your site map generator logic will go.
