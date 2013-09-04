# Entity Type Selection

When a user clicks on the "Add" button on a main admin list grid screen for an entity that has been extended, they will typically be prompted to select what discrete type they want to create. For example, if you've extended CategoryImpl to MyCategoryImpl, the user will be asked to choose if they want to create a CategoryImpl or a MyCategoryImpl.

Broadleaf provides two different hooks for limiting this selection dialog for users. The first hook will allow you to bypass that screen entirely and automatically select a discrete polymorphic type for the user. The second hook will allow you to limit the selection to a specific list of entities.

## Basic Setup (for both hooks)

1. Create a controller for the entity type you'd like to control. We will be using ProductOptions for this example. Note that the getSectionKey method should be implemented as follows.

    ```java
    package com.mycompany.admin.web.controller;

    @Controller
    @RequestMapping("/" + MyAdminProductOptionController.SECTION_KEY)
    public class MyAdminProductOptionController extends AdminBasicEntityController {

        protected static final String SECTION_KEY = "product-options";

        @Override
        protected String getSectionKey(Map<String, String> pathVars) {
            if (super.getSectionKey(pathVars) != null) {
                return super.getSectionKey(pathVars);
            }
            return SECTION_KEY;
        }

    }
    ```

2. Ensure that your controller is scanned in the `applicationContext-servlet-admin.xml` file:

    ```xml
    <context:component-scan base-package="com.mycompany.admin.web" />
    ```

    > The typical convention is to scan as far as the `web` package. This will let you have a nice separation of what gets scanned in the servlet vs non-servlet context files.

## Hook 1: Select an entity for the user automatically

This is the easier of the two hooks. Simply provide the following method in your controller:

```java
protected String getDefaultEntityType() {
    return MyProductOptionImpl.class.getName();
}
```

And that's it! Broadleaf will now automatically select the MyProductOptionImpl version of the class.

## Hook 2: Limit the choices for a user

This is similar to hook 1, but we'll be overriding a different method:

```java
protected List<ClassTree> getAddEntityTypes(ClassTree classTree) {
    // Your implementation goes here. The return list is what will be used 
    // to generate the menu. For reference, the default Broadleaf implementation
    // is: return classTree.getCollapsedClassTrees();
}
```

In here, you'll be responsible for returning a list of ClassTree objects from the source one. The returned list is what will be used to generate the available options for the user. The typically implementation would be to call `classTree.getCollapsedClassTrees()`, iterate through the returned list, and remove the elements that you do not want shown.

