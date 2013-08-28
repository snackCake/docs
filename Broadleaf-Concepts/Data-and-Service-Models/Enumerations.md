#Enumerations

Broadleaf provides two data structures to store enumeration like data, a `BroadleafEnumerationType` and a `DataDrivenEnumeration`.  Both are, in essence, just a collection of `String` values.  A `BroadleafEnumerationType` is used to provide a predefined set of values to use in business logic, while a `DataDrivenEnumerations` is used to provide a variable set of values to use in places like drop-down menus in the admin console.

##BroadleafEnumerationType

The `BroadleafEnumerationType` is an interface that is implemented to create a custom enumeration of predefined hard-coded `String` values.  An example from the Broadleaf framework would be `FulfillmentType`.  `FulfillmentType` is an enumeration that contains values corresponding to the available `FulfillmentGroup` types.  Here is how `BroadleafEnumerationType` is implemented by `FulfillmentType`:

```java
public class FulfillmentType implements Serializable, BroadleafEnumerationType {

    private static final long serialVersionUID = 1L;

    private static final Map<String, FulfillmentType> TYPES = new LinkedHashMap<String, FulfillmentType>();

    public static final FulfillmentType DIGITAL = new FulfillmentType("DIGITAL", "Digital");
    public static final FulfillmentType PHYSICAL_SHIP = new FulfillmentType("PHYSICAL_SHIP", "Physical Ship");
    public static final FulfillmentType PHYSICAL_PICKUP = new FulfillmentType("PHYSICAL_PICKUP", "Physical Pickup");
    public static final FulfillmentType PHYSICAL_PICKUP_OR_SHIP = new FulfillmentType("PHYSICAL_PICKUP_OR_SHIP", "Physical Pickup or Ship");
    public static final FulfillmentType GIFT_CARD = new FulfillmentType("GIFT_CARD", "Gift Card");
    @Deprecated
    public static final FulfillmentType SHIPPING = new FulfillmentType("SHIPPING", "Shipping");

    public static FulfillmentType getInstance(final String type) {
        return TYPES.get(type);
    }

    private String type;
    private String friendlyType;
```

and here is an example of how `FulfillmentType` is used in business logic:

```java
if(!FulfillmentType.GIFT_CARD.equals(fulfillmentGroup.getType())) {
    shippableFulfillmentGroups++;
}
```

##DataDrivenEnumeration

A `DataDrivenEnumeration` is an entity that contains a list of `DataDrivenEnumerationValue` values.  A `DataDrivenEnumerationValue` is also an entity and provides structure to a single `String` value.  In contrast to the `BroadleafEnumerationType` which is used to hard-code a set of values, the `DataDrivenEnumeration` contains a set of database stored values which can be changed as needed.

The admin console provides configuration screens to create and edit `DataDrivenEnumerations` (under the `Utilities` tab in the sidebar).  Once an enumeration is created using the admin console, it is saved to the database for future use.  
  
An example of how use a `DataDrivenEnumeration` is the creation of drop-down menus in the admin console.  In order to use a `DataDrivenEnumeration` to supply values to an admin drop-down menu, the `@AdminPresentation` annotation is used with the `@AdminPresentationDataDrivenEnumeration` annotation.  Below is an example:

>Note:  `@AdminPresentation` is used to style and place the field.  `@AdminPresentationDataDrivenEnumeration` is used to specify that the field will be assigned a value from a specific subset of `DataDrivenEnumerationValue` values.  

```java
@Column(name = "TAX_CODE")
@AdminPresentation(friendlyName = "SkuImpl_Sku_TaxCode")
@AdminPresentationDataDrivenEnumeration(optionFilterParams = { @OptionFilterParam(param = "type.key", value = "TAX_CODE", paramType = OptionFilterParamType.STRING) })
protected String taxCode;
```
+ `optionFilterParams` - Additional parameters to refine the query that is used to specify which values will be visible in the drop-down menu.
+ `param = "type.key"` - The field name in the target entity that should be used to refine the query.  In this case, type is from the `DataDrivenEnumerationValue` and key is from `DataDrivenEnumeration`.
+ `value = "TAX_CODE"` - The field value that should match for any items returned from the query.
+ `paramType = OptionFilterParamType.STRING` - This is the type for the value stored in this `OptionFilterParam` annotation.

The `@AdminPresentationDataDrivenEnumeration` annotation specifies that the field `taxCode` will be assigned a value from the set of `DataDrivenEnumerationValue` values which have `type.key = "TAX_CODE"`.  The admin console will display a drop-down menu containing this set of values.

Here is alternate example illustrating the creation of a drop-down whose values come from `CategoryImpl.name`:

```java
@Column(name = "NAME")
@AdminPresentation(friendlyName = "CategoryImpl_Category_Name")
@AdminPresentationDataDrivenEnumeration(optionListEntity = CategoryImpl.class, optionValueFieldName = "name", 
        optionDisplayFieldName = "name", optionCanEditValues = true)
protected String name;
```

+ `optionListEntity` - Specifies the target entity that should be queried for the list of options that will be presented to the user in a drop-down list. In this case, it will use `CategoryImpl.class`.
+ `optionValueFieldName = "name"` - Specify the field in the target entity that contains the value that will be persisted into this annotated field.  In this case, it will use `CategoryImpl.name`.
+ `optionDisplayFieldName = "name"` - Specify the field in the target entity that contains the display value that will be shown to the user in the drop-down field. In this case, it will use `CategoryImpl.name`.
+ `optionCanEditValues = true` - Whether or not the user can edit (or enter new values) in the drop-down menu.

The `@AdminPresentationDataDrivenEnumeration` annotation specifies that the field name will be assigned a value from the set of all `CategoryImpl.name` values (no `optionFilterParams` were used to refine the query).





