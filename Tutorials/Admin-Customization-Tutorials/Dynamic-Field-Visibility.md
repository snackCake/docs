# Dynamic Field Visibility

To control the visibility of an admin field based on the value of another admin field, the following API can be used:

```js
var className = 'org.broadleafcommerce.core.catalog.domain.Product';
var parent = '#field-defaultSku--taxable';
var child = '#field-defaultSku--taxCode';
var valueToShowFor = 'true';

BLCAdmin.addDependentFieldHandler(className, parent, child, valueToShowFor);
```

Let's examine what's happening. First, we're restricting this behavior to the Product entity. Next, we're specifying two different jQuery selectors that represent the parent and child fields. Lastly, we're providing the value that the parent field must have for the child field to be visible.

If the behavior you need to control the field visibility is more complex, you can alternatively pass in a function for the last argument. This function should accept one argument, which will be the value of the parent field. The function should return true if the child field should be visibile, and false otherwise.

