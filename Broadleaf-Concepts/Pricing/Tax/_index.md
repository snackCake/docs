# Tax

Broadleaf Commerce can be configured for tax calculation in a number of ways. You can use the simple tax provider or  create your own custom tax provider. No tax provider is hooked up out of the box, but it is pretty trivial to hook one up yourself.

## Tax Service

The `TaxService` interface is a component that is designed to hold a list of `TaxProviders`. Both interfaces have the following methods:

```java
public Order calculateTaxForOrder(Order order, ModuleConfiguration config) throws TaxException;

public Order commitTaxForOrder(Order order, ModuleConfiguration config) throws TaxException;

public void cancelTax(Order order, ModuleConfiguration config) throws TaxException;
```

## Simple Tax Provider

The simple tax provider is a good option for companies with simple tax needs. It allows you to configure a tax rate that applies to an entire site or a tax rate for specific postal-codes, cities, states, or countries.

To find out more about the `SimpleTaxProvider`, see the [[Simple Tax Module Configuration | Simple Tax Provider]] section.

## Creating Your Own Tax Provider

If the provided modules in Broadleaf do not meet your needs, you can easily write a customized tax module to integrate with your tax calculator. Please view the [[Creating a Tax Provider]] section.

