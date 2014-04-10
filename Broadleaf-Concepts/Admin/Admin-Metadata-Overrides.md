# Admin Metadata Overrides

All of the form fields in the admin are built dynamically using `@AdminPresentation` annotations. All of the Broadleaf framework entities are mmarked up with their own annotations to give a good base line for how forms are displayed.

However, you might want to customize how these are presented depending on your needs. This can be achieved in a couple of different ways:

1. XML-based overrides
2. Annotation-based overrides (only applicable for extended entities)

## Annotation-based overrides

If you have extended one of the existing Broadleaf entities then you can modify the display attributes of the Broadleaf superclass. Let's use and extension of `Product` as an example:

```java
@Entity
@Table(name = "MYCOMPANY_PRODUCT")
public MyCompanyProduct extends ProductImpl {

    @Column(name = "CUSTOM_FIELD")
    @AdminPresentation(friendlyName = "My Custom Field")
    protected String someCustomField;
}
```

The field annotated with `@AdminPresentation` is merged with all of the other fields from the `ProductImpl` superclass and displays in forms wherever a product is.

You might now want to override what happens with the `description` field as well. This is a nested property on the `defaultSku` property of a Product, and is excluded out of the box in Broadleaf. This is what the annotation looks like as of Broadleaf 3.1.1-GA:

```java
@Column(name = "DESCRIPTION")
@AdminPresentation(friendlyName = "SkuImpl_Sku_Description", order = ProductImpl.Presentation.FieldOrder.SHORT_DESCRIPTION, 
    group = ProductImpl.Presentation.Group.Name.General, groupOrder = ProductImpl.Presentation.Group.Order.General,
    largeEntry = true, 
    excluded = true,
    translatable = true)
protected String description;
```

Notice the attribute `excluded = true`. But what if you want to show this field?

Enter `@AdminPresentationMergeOverride`. Let's look at what is required to override this field and then break it down as to what is going on:

```java
@Entity
@Table(name = "MYCOMPANY_PRODUCT")
@AdminPresentationMergeOverrides({
    @AdminPresentationMergeOverride(name = "defaultSku.description", mergeEntries = {
        @AdminPresentationMergeEntry(propertyType = PropertyType.AdminPresentation.EXCLUDED, booleanOverrideValue=false)
    })
})
public class MyCompanyProduct extends ProductImpl {
```

* `@AdminPresentationMergeOverrides` - class level starting-point annotation that contains a list of `@AdminPresentationMergeOverride`
* `@AdminPresentationMergeOverride` - unique based on the property `name` that you are trying to override. In the example above you can see that this supports dot-notation to target fields in related entities

    > Dot-syntax is only available to `@ManyToOne` and `@OneToOne` relationships on entities with `@AdminPresentationClass(populateToOneFields = PopulateToOneFieldsEnum.TRUE)`

* `mergeEntries` - all of the overrides to the various properties of `@AdminPresentation` that were defined in the superclass
* `@AdminPresentationMergeEntry` - while the `propertyType` attribute accepts a String, to be more explicit you should use the `PropertyType` static class to traverse the `@AdminPresentation` attribute that you are trying to override.
* `overrideValue` - the `@AdminPresentationMergeEntry` has different value properties depending on the Java type of the corresponding `@AdminPresentation` attribute (like `intOverrideValue`, `overrideValue`, `longOverrideValue`, etc). Which one you use depends on the type of the attribute you are overriding. In this example, the `excluded` attribute accepts a boolean so we use `booleanOverrideValue`

What about if you also wanted to make the description field a WYSIWYG HTML editor?

```java
@Entity
@Table(name = "MYCOMPANY_PRODUCT")
@AdminPresentationMergeOverrides({
    @AdminPresentationMergeOverride(name = "defaultSku.description", mergeEntries = {
        @AdminPresentationMergeEntry(propertyType = PropertyType.AdminPresentation.EXCLUDED, booleanOverrideValue=false),
        @AdminPresentationMergeEntry(propertyType = PropertyType.AdminPresentation.FIELDTYPE, overrideValue="HTML_BASIC")
    })
})
public class MyCompanyProduct extends ProductImpl {
```

And finally, what if we wanted to excluded the URL field from displaying in the admin at all?

```java
@Entity
@Table(name = "MYCOMPANY_PRODUCT")
@AdminPresentationMergeOverrides({
    @AdminPresentationMergeOverride(name = "defaultSku.description", mergeEntries = {
        @AdminPresentationMergeEntry(propertyType = PropertyType.AdminPresentation.EXCLUDED, booleanOverrideValue=false),
        @AdminPresentationMergeEntry(propertyType = PropertyType.AdminPresentation.FIELDTYPE, overrideValue="HTML_BASIC")
    }),
    @AdminPresentationMergeOverride(name = "url", mergeEntries = {
        @AdminPresentationMergeEntry(propertyType = PropertyType.AdminPresentation.EXCLUDED, booleanOverrideValue=true)
    })
})
public class MyCompanyProduct extends ProductImpl {
```

## XML-based overrides

In some instances, you may find that the Broadleaf out of the box entities are sufficient for your needs but you still want to override some of the display attributes of the entity properties themselves.

This is acheivable through the `mo:override` XML namespace. To utilize the namespace, you need to make sure that you have the proper `schemaLocation` and `xmlns` difined within the `<beans>` element in your `applicationContext`:

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       ...
       ...
       ...
       xmlns:mo="http://schema.broadleafcommerce.org/mo"
       ...
       xsi:schemaLocation="
          http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
          ...
          ...
          http://schema.broadleafcommerce.org/mo
          http://schema.broadleafcommerce.org/mo/mo-3.0.xsd">

```

> The web URL schema.broadleafcommerce.org is an internal address resolved through the Broadleaf jars

 
