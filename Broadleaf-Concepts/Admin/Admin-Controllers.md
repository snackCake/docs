# Admin Controllers

The admin application was written in a such a way that it is possible to override as much or as little functionality as you want for a specific entity. However, controller inheritance is very nuanced in Spring MVC, and there are a few important steps to follow when attempting to extend a Broadleaf admin controller.

There are two different scenarios that determine exactly what to do to achieve controller inheritance. The first case occurs when you are attempting to override a method that a section-specific Broadleaf admin controller already overrides. For example, consider the following code snippets:

In `AdminBasicEntityController`:

```java
@RequestMapping(value = "/{id}/{collectionField:.*}/{collectionItemId}", ...)
public String showUpdateCollectionItem( ... )
```

In `AdminProductController`:

```java
@RequestMapping(value = "/{id}/{collectionField:.*}/{collectionItemId}", ...)
public String showUpdateCollectionItem( ... )
```

If you were trying to customize `showUpdateCollectionItem()` for products, you are **overriding an overridden request mapping**

The second scenario would be attempting to override `viewEntityList()` from `AdminBasicEntityController` for products. In this case, `AdminProductController` doesn't already override this method, and you are thus **overriding a non-overriden request mapping**.

If you are going to be extending `AdminBasicEntityController` directly because Broadleaf does not already provide a section specific controller for your entity, you can use `AdminProductController` as a model for your desired behavior and disregard the rest of this document. Please do note that the `getSectionKey()` method as seen in `AdminProductController` is required and must be copied over to your controller.

The initial steps are the same regardless of the override type you're attempting:

1. Create a Java class that extends the appropriate Broadleaf section controller, such as `AdminProductController`.

2. In `applicationContext-servlet-admin.xml`, add a bean override for that controller's bean id to your specific class.

3. Ensure that your class **does not** have `@Controller` or a class level `@RequestMapping` annotation

4. Override the method you need, keeping all parameter-level annotations (such as `@PathVariable`)

5. Perform whatever logic you need. Typically, this might involve a call to the super method, doing some extra stuff, and returning what super returned. However, you're of course free to do whatever you'd like.

Now, you must make one small change depending on whether or not you're overriding a previously overridden request mapping or not.

## Overriding an overridden request mapping

1. Remove the `@RequestMapping` annotation from the method.

## Overriding a non-overridden request mapping

1. Ensure the method does have an `@RequestMapping` annotation.
