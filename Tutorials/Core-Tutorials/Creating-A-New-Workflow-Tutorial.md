# Creating A New Workflow Tutorial

## Overview

In this tutorial, we're going to examine adding a custom workflow that leverages the powerful `SequenceProcessor` in Broadleaf. We will describe the steps necessary to create the `MyAwesomeAddItemWorkflow` that will perform an inventory check, and if successful add the item to the cart. This is similar to the already existing `blAddItemWorkflow` but will be easier to follow along for the tutorial.

> **NOTE**: The workflow we're creating is not actually awesome. You would never actually want to use this particular workflow in your application -- Broadleaf already provides workflows that are utilized for adding / updating / removing items from cart. We're just using this as a basis for a tutorial that demonstrates creating custom workflows. Learn more about the [[Cart Operations]].

We'll start by examining the XML configuration piece that we will need to activate the workflow. After that, we'll go ahead and implement all of the necessary elements defined in the configuration and test it out. Additional documentation can be found at the [[Workflows and Activities]] page.

## XML Configuration

Here's the chunk of code we're going to need:

```xml
<bean id="myAwesomeAddItemWorkflow" class="org.broadleafcommerce.core.workflow.SequenceProcessor">
    <property name="processContextFactory">
        <bean class="com.mycompany.core.order.service.workflow.AddItemProcessContextFactory"/>
    </property>
    <property name="activities">
        <list>
            <bean p:order="100" class="com.mycompany.core.order.service.workflow.CheckInventoryActivity"/>
            <bean p:order="200" class="com.mycompany.core.order.service.workflow.AddOrderItemActivity"/>
        </list>
    </property>
    <property name="defaultErrorHandler" ref="blDefaultErrorHandler"/>
</bean>
```

Let's take a look at the important things here:

- The bean we're instantiating is an instance of the `SequenceProcessor` class and we're calling it `myAwesomeAddItemWorkflow`
- We're defining something that subclasses `ProcessContextFactory` and in this example we're calling it `AddItemProcessContextFactory`
- We added two activities: `CheckInventoryActivity` and `AddOrderItemActivity`, which do what you would expect

OK, awesome. That configuration was really simple and straight forward, and now all we have to do is implement our classes.

## Defining the ProcessContext and the ProcessContextFactory

Before we can create our custom implementation of a `ProcessContextFactory`, we have to have a custom `ProcessContext` for it to work with. 

Let's go ahead and create an our context:

```java
public class AddItemSeed {

    protected OrderItemRequest orderItemRequest;
    protected Order order;

    public AddItemSeed(OrderItemRequest request, Order order) {
        this.request = request;
        this.order = order;
    }

    public Order getOrder() {
        return order;
    }

    public void setOrder(Order order) {
        this.order = order;
    }

    public OrderItemRequest getRequest() {
        return request;
    }

    public void setRequest(OrderItemRequest request) {
        this.request = request;
    }

}
```

Boom, done. Let's take a look at what was important

- We have a generic DTO seed class that will be passed around to the workflow. This stores all of the information required by all of the activities in the workflow to properly execute.


And now we can create our factory:

```java
public class AddItemProcessContextFactory implements ProcessContextFactory<AddItemSeed, AddItemSeed> {

    public ProcessContext<AddItemSeed> createContext(AddItemSeed seedData) throws WorkflowException {
        ProcessContext<AddItemSeed> context = new DefaultProcessContextImpl<AddItemSeed>();
        context.setSeedData(seedData);
        return context;
    }
}
```

Nothing special here, we're just creating an instance of a generic context with the seed data.

## The Activities

Recall that in our XML we defined two activities for our `AddItemWorkflow`

```xml
<bean p:order="100" class="com.mycompany.core.order.service.workflow.CheckInventoryActivity"/>
<bean p:order="200" class="com.mycompany.core.order.service.workflow.AddOrderItemActivity"/>
```

Let's implement them!

```java
public class CheckInventoryActivity extends BaseActivity<ProcessContext<AddItemSeed>> {
    private static Log LOG = LogFactory.getLog(AddOrderItemActivity.class);
    
    public ProcessContext<AddItemSeed> execute(ProcessContext<AddItemSeed> context) throws Exception {
        OrderItemRequest orderItemRequest = context.getSeedData().getRequest();

        ... logic to check inventory for the current item ...
        ... if no inventory, you would throw an exception ...
        
        return context;
    }
}
```

```java
public class AddOrderItemActivity extends BaseActivity<ProcessContext<AddItemSeed>> {
    private static Log LOG = LogFactory.getLog(AddOrderItemActivity.class);
    
    
    public ProcessContext<AddItemSeed> execute(ProcessContext<AddItemSeed> context) throws Exception {
        AddItemSeed seed = context.getSeedData();
        OrderItemRequest orderItemRequest = seed.getRequest();
        Order order = seed.getOrder();

        ... logic to add the order item to the current order ...
        
        // if we saved the order, put the saved instance back on the seed
        seed.setOrder(order);
        return context;
    }
}
```

Review time! What did we do here? We created two different activities, `CheckInventoryActivity` and `AddOrderItemActivity` that both extended `BaseActivity`. Each activity grabbed what it needed from the `seedData`, performed its required step (throwing an exception if it failed) and then propagated the context forward to the next activity. The last activity (`AddOrderItemActivity`) would propagate its context to the caller. This means that you are allowed to change the context at will in the activities!

## Tying it all together

One last thing -- actually executing this workflow! Let's take a look:

```java

@Resource(name = "blOrderService")
protected OrderService orderService;

@Resource(name = "myAwesomeAddItemWorkflow")
protected SequenceProcessor myAwesomeAddItemWorkflow;

public boolean addItem(OrderItemRequest orderItemRequest, Long orderId) {
    Order order = orderService.findOrderById(orderId);

    AddItemSeed seed = new AddItemSeed(orderItemRequest, order);

    try {
        ProcessContext<AddItemSeed> context = (ProcessContext<AddItemSeed>) myAwesomeAddItemWorkflow.doActivities(seed);
        return true;
    } catch (WorkflowException e) {
        return false;
    }
}
```

That's it! We create our object for the `seedData` and invoke the workflow! Any changes to the the `AddItemSeed` would be returned by the workflow execution and could be utilized after it's complete.
