# Workflows and Activities

Broadleaf provides configurable workflows for key phases of the eCommerce process - namely: checkout, pricing and cart operations. These workflows are represented in xml in the Spring application context file. At the most basic level, Broadleaf provides a default configuration for checkout that covers the basic steps using simple modules. Most users will want to override part, or all, of the steps defined in these default configurations to fit their own needs and business rules. We'll start out by describing the default configuration, and then advance later into customization strategies.

## Anatomy of a workflow

```xml
    <!-- Pricing Workflow configuration -->
    <bean p:order="1000" id="blOfferActivity" class="org.broadleafcommerce.core.pricing.service.workflow.OfferActivity" />
    <bean p:order="2000" id="blConsolidateFulfillmentFeesActivity" class="org.broadleafcommerce.core.pricing.service.workflow.ConsolidateFulfillmentFeesActivity" />
    <bean p:order="3000" id="blFulfillmentItemPricingActivity" class="org.broadleafcommerce.core.pricing.service.workflow.FulfillmentItemPricingActivity" />
    <bean p:order="4000" id="blFulfillmentGroupMerchandiseTotalActivity" class="org.broadleafcommerce.core.pricing.service.workflow.FulfillmentGroupMerchandiseTotalActivity" />
    <bean p:order="5000" id="blFulfillmentGroupPricingActivity" class="org.broadleafcommerce.core.pricing.service.workflow.FulfillmentGroupPricingActivity" />
    <bean p:order="6000" id="blShippingOfferActivity" class="org.broadleafcommerce.core.pricing.service.workflow.ShippingOfferActivity" />
    <bean p:order="7000" id="blTaxActivity" class="org.broadleafcommerce.core.pricing.service.workflow.TaxActivity" />
    <bean p:order="8000" id="blTotalActivity" class="org.broadleafcommerce.core.pricing.service.workflow.TotalActivity" />
    <bean p:order="9000" id="blAdjustOrderPaymentsActivity" class="org.broadleafcommerce.core.pricing.service.workflow.AdjustOrderPaymentsActivity" />


    <bean id="blPricingWorkflow" class="org.broadleafcommerce.core.workflow.SequenceProcessor">
        <property name="processContextFactory">
            <bean class="org.broadleafcommerce.core.pricing.service.workflow.PricingProcessContextFactory"/>
        </property>
        <property name="activities">
            <list>
                <ref bean="blOfferActivity" />
                <ref bean="blConsolidateFulfillmentFeesActivity" />
                <ref bean="blFulfillmentItemPricingActivity" />
                <ref bean="blFulfillmentGroupMerchandiseTotalActivity" />
                <ref bean="blFulfillmentGroupPricingActivity" />
                <ref bean="blShippingOfferActivity" />
                <ref bean="blTaxActivity" />
                <ref bean="blTotalActivity"/>
                <ref bean="blAdjustOrderPaymentsActivity"/>
            </list>
        </property>
        <property name="defaultErrorHandler">
            <bean class="org.broadleafcommerce.core.workflow.DefaultErrorHandler">
                <property name="unloggedExceptionClasses">
                    <list>
                        <value>org.hibernate.exception.LockAcquisitionException</value>
                    </list>
                </property>
            </bean>
        </property>
    </bean>
```

Every workflow is actually an instance of the `SequenceProcessor` class from Broadleaf. This class manages the orderly activation of subordinate activities as well handling error states, should they occur. Workflows are configured in three main areas, the process context factory, the activities list, and the rollback handler.

### ProcessContextFactory
The `processContextFactory` property must be set with an instance of a class implementing the `ProcessContextFactory` interface. All such implementers are responsible for creating an instance of a class implementing the `ProcessContext` interface (more on this in a bit). In our example, the `PricingProcessContextFactory` class is used, which creates an instance of the `ProcessContext<Order>`.

> ProcessContexts are usually only relevant for a particular type of workflow, and thus there is usually a different ProcessContextFactory for each workflow as well. An immediate exception to this rule is that each of the cart workflows (add, remove, update) all use the same `ProcessContext` and thus the same `ProcessContextFactory` since they are so similar.

### Activities
The `activities` property must be filled with a list containing one or more `Activities`. Each `Activity` in the workflow is responsible for performing a single unit of work (like computing tax in the `TaxActivity` or summing up the total price of an order in the `TotalActivity`). Composite activities can contain a subordinate workflow, allowing us to create complex, nested workflows. We'll talk more about composite activities later.

### Error Handler
The error handler is an ErrorHandler instance passed to the defaultErrorHandler property. By default, we specify the blDefaultErrorHandler, which is a simple error handler bean defined in the Broadleaf Commerce application context. This default error handler only serves to log the exception to system.out and then bubbles the exception - stopping this workflow and its processing of activities for the current thread. We'll talk a little more about error handlers later.

## Activity Ordering
If you look back at that initial reference to the `blPaymentWorkflow` at the beginning of this document, you'll notice a slightly peculiar configuration for the activity:

```xml
<bean p:order="1000" id="blOfferActivity" class="org.broadleafcommerce.core.pricing.service.workflow.OfferActivity" />
```

The `blOfferActivity` has a `p:order` property defined. This will set the `order` property for this `Activity`, which determines how the framework will order these activities in relation to other activities. This is really handy when combined with the fact that Broadleaf merges activity lists together within the same bean id. Let's look at a more complicated example of ordering in action with the `blUpdateItemWorkflow`:

```xml
<bean p:order="1000" id="blVerifyCustomerMaxOfferUsesActivity" class="org.broadleafcommerce.core.offer.service.workflow.VerifyCustomerMaxOfferUsesActivity"/>
<bean p:order="2000" id="blPaymentServiceActivity" class="org.broadleafcommerce.core.checkout.service.workflow.PaymentServiceActivity"/>
<bean p:order="3000" id="blRecordOfferUsageActivity" class="org.broadleafcommerce.core.offer.service.workflow.RecordOfferUsageActivity"/>
<bean p:order="4000" id="blCompleteOrderActivity" class="org.broadleafcommerce.core.checkout.service.workflow.CompleteOrderActivity"/>

<bean id="blCheckoutWorkflow" class="org.broadleafcommerce.core.workflow.SequenceProcessor">
    <property name="processContextFactory">
        <bean class="org.broadleafcommerce.core.checkout.service.workflow.CheckoutProcessContextFactory"/>
    </property>
    <property name="activities">
        <list>
            <ref bean="blVerifyCustomerMaxOfferUsesActivity" />
            <ref bean="blPaymentServiceActivity" />
            <ref bean="blRecordOfferUsageActivity" />
            <ref bean="blCompleteOrderActivity" />
        </list>
    </property>
    <property name="defaultErrorHandler" ref="blDefaultErrorHandler"/>
</bean>
```

In all Broadleaf workflows each framework-defined activity goes up by 1000, allowing you to order your own activities in-between. For instance, if you want to put a custom activity in-between the recording offer usage and marking the order as completed, you would define the following in your applicationContext-mycompany.xml:

```xml
<bean id="blCheckoutWorkflow" class="org.broadleafcommerce.core.workflow.SequenceProcessor">
    <property name="activities">
        <list>
            <bean p:order="3500" class="com.mycompany.core.workflow.DecrementInventoryActivity" />
        </list>
    </property>
</bean>
```

A few important notes about this activity ordering:
- If 2 activities have the exact same order (for instance, both are configured with '3000') then the ordering will be **in-place**. This means that the ordering will be determined by applicationContext merge ordering (as defined in the `patchConfigLocations` in web.xml)
- Integration modules should declare their activity ordering in the 100 range (like 3100, 3200, etc) so that specific implementations can further weave custom activities in-between those as well
- All framework activity ordering can be overridden by referencing the bean id and changing the `p:order` attribute
- **If you have not configured your activity to have an explicit ordering, your activity will be placed at the end of the workflow (more explicitly, the default order for activities is `Ordered.LOWEST_PRECEDENCE`)**

## Rollback Handlers

Rollback handlers provide a way for an activity to register state, and possibly rollback an operation at some later time. Consider this example, one of the first steps of the checkout workflow would be validate and confirm any payments on the Order. After payments have been confirmed and charged, a following step may update the cart status or send a notification to a downstream system. Now, if for some reason the cart status update step fails, it would be useful to have a standardized way to refund or void the credit card charge made previously. This is the purpose of the rollback handler.

Rollback handlers can perform any custom code desired and can operate based on the state that is passed in. As a result, the rollback operation doesn't have to be to an external service like a payment gateway. It could instead be a [compensating transaction](http://en.wikipedia.org/wiki/Compensating_transaction) that returns the database state back to what it was previous to the activity's execution. This is an important distinction, as most workflows themselves are NOT executed under a JDBC transaction, as the workflow lifecycle length makes keeping transactions open impractical. Since the whole workflow is not under a single transaction, there will not be an immediate and automatic rollback of database state upon exception, which mean that a rollback handler must be used to explicitly set the database back to the desired state.

The simplest form of rollback handler is configured via xml application context in the activity declaration for the workflow. Let's look at the checkout workflow declaration:

```xml
    <bean p:order="1000" id="blVerifyCustomerMaxOfferUsesActivity" class="org.broadleafcommerce.core.offer.service.workflow.VerifyCustomerMaxOfferUsesActivity"/>
    <bean p:order="2000" id="blValidateProductOptionsActivity" class="org.broadleafcommerce.core.checkout.service.workflow.ValidateProductOptionsActivity"/>
    <bean p:order="3000" id="blValidateAndConfirmPaymentActivity" class="org.broadleafcommerce.core.checkout.service.workflow.ValidateAndConfirmPaymentActivity">
        <property name="rollbackHandler" ref="blConfirmPaymentsRollbackHandler" />
    </bean>
    <bean p:order="4000" id="blRecordOfferUsageActivity" class="org.broadleafcommerce.core.offer.service.workflow.RecordOfferUsageActivity">
        <property name="rollbackHandler" ref="blRecordOfferUsageRollbackHandler" />
    </bean>
    <bean p:order="5000" id="blCommitTaxActivity" class="org.broadleafcommerce.core.checkout.service.workflow.CommitTaxActivity">
        <property name="rollbackHandler" ref="blCommitTaxRollbackHandler" />
    </bean>
    <bean p:order="6000" id="blDecrementInventoryActivity" class="org.broadleafcommerce.core.checkout.service.workflow.DecrementInventoryActivity">
        <property name="rollbackHandler" ref="blDecrementInventoryRollbackHandler" />
    </bean>
    <bean p:order="7000" id="blCompleteOrderActivity" class="org.broadleafcommerce.core.checkout.service.workflow.CompleteOrderActivity">
        <property name="rollbackHandler" ref="blCompleteOrderRollbackHandler" />
    </bean>

    <bean id="blCheckoutWorkflow" class="org.broadleafcommerce.core.workflow.SequenceProcessor">
        <property name="processContextFactory">
            <bean class="org.broadleafcommerce.core.checkout.service.workflow.CheckoutProcessContextFactory"/>
        </property>
        <property name="activities">
            <list>
                <ref bean="blVerifyCustomerMaxOfferUsesActivity" />
                <ref bean="blValidateProductOptionsActivity" />
                <ref bean="blValidateAndConfirmPaymentActivity" />
                <ref bean="blRecordOfferUsageActivity" />
                <ref bean="blCommitTaxActivity" />
                <ref bean="blDecrementInventoryActivity" />
                <ref bean="blCompleteOrderActivity" />
            </list>
        </property>
        <property name="defaultErrorHandler">
            <bean class="org.broadleafcommerce.core.workflow.DefaultErrorHandler">
                <property name="unloggedExceptionClasses">
                    <list>
                        <value>org.broadleafcommerce.core.inventory.service.InventoryUnavailableException</value>
                    </list>
                </property>
            </bean>
        </property>
    </bean>
```

In the out-of-box Checkout workflow, notice we include several rollback handlers that will execute if there is an exception in any of the subsequent activities. (Note, an activity's own rollback handler is not called when that activity fails, just the rollback handlers of those activities that have already succeeded previously in the workflow. It is the activity's responsibility to rectify its own state, if applicable, when it fails). In the example above, if the cart update step fails, the `blConfirmPaymentsRollbackHandler` will get involved which will call the configured Payment Gateway to either refund or void the payment based on implementation.

### RollbackHandler Interface

All rollback handlers implement the RollbackHandler interface, which has a single method that must be implemented

```java
public void rollbackState(Activity<? extends ProcessContext> activity, ProcessContext processContext, Map<String, Object> stateConfiguration) throws RollbackFailureException;
```

The contract contains all the information that should be necessary to perform the compensating transaction. The activity in question, as well as the ProcessContext (see next section) are provided. Most important, the stateConfiguration map is provided that contains important information about the operation that was previously performed by the activity. This state information can be used to help reverse the operation. The stateConfiguration is generally provided by the activity during execution, which we'll look at next.

### ActivityStateManager

The ActivityStateManager is a flexible component used to register a rollback handler and any accompanying state. The ActivityStateManager can be called from anywhere in the flow of execution for a workflow (Activity, Module, ErrorHandler, etc...). To get a handle on the singleton ActivityStateManager instance, execute:

```java
ActivityStateManagerImpl.getStateManager();
```

The ActivityStateManager interface provides several methods for registering RollbackHandlers and state. In fact, you can even further refine rollback behavior by calling the overloaded version of registerState that accepts a region parameter. Region allows you to provide a grouping label for one or more RollbackHandlers, which provides an avenue for selectively rolling back a subset of registered rollback handlers. ActivityStateManager also has methods for clearing state and for executing rollbacks, which may also be called at any time (although you'll generally let the system call these latter methods for you automatically).

### Implicit vs Explicit Registration

By default, one must explicitly register a RollbackHandler in code (see above). This is because most RollbackHandlers will require thread specific state to be registered in order to achieve a viable rollback state. However, for more simple rollback handlers, if no thread specific state is required to achieve a rollback, then the system can be configured for implicit/automatic rollback registration. For example:

```xml
<bean id="myActivity"
      class="com.test.MyActivity">
    <property name="rollbackHandler" ref="blTestRollbackHandler"/>
    <property name="stateConfiguration">
        <map>
            <entry key="testProperty" value="testValue"/>
        </map>
    </property>
    <property name="automaticallyRegisterRollbackHandler" value="true"/>
</bean>
```

In this example, we're specifying a rollbackHandler, some static state and telling the system to automatically register the RollbackHandler. Because we don't care about thread specific state here, this config is all that's required to register the RollbackHandler with the ActivityStateManager singelton.

### Implicit vs Explicit Rollback

By default, registered RollbackHandler instances are automatically executed upon exception in the workflow execution. However, there may be cases where it is advantageous to turn off this behavior and instead to call rollback explicitly from ActivityStateManager in code (e.g. you might want to rollback a specific region, instead of all rollback handlers). To engage explicit rollback behavior, declare

```ini
workflow.auto.rollback.on.error=false
```

in your runtime environment configuration property files. Note, this behavior can be overridden for each workflow by setting the autoRollbackOnError property for each workflow in your app context xml.

## ProcessContext

A ProcessContext is a common context object that is passed in and out of every activity in a workflow. This context object usually contains data pertinent to the theme of the workflow. Each ProcessContext provides methods to set and check the state of the workflow using `stopProcess` and `isStopped` methods, respectively. For example, calling stopProcess directly, an activity could stop further processing of a workflow without necessarily raising an exception.

### Conditional Activity Execution
The `Activity` interface provides support for skipping over that activity based on the items in the `ProcessContext`. This method is invoked before executing each `Activity`. This could prevent duplication of workflow definitions; perhaps a single large workflow could be declared for multiple configurations of a `ProcessContext`.

## Activities

Activities in their most basic state are instances of the `Activity` interface, which provides simple entry points for executing the activity and retrieving the error handler. Most activities will actually implement the `BaseActivity` abstract class.

An `Activity` is also only relevant for a certain type of workflow (meaning it can only operate on a certain type of `ProcessContext`) defined by Java generics. For instance, this would the the definition of an activity in the `blPricingWorkflow`:

```java
public class TotalActivity extends BaseActivity<PricingContext> {

    @Override
    public PricingContext execute(PricingContext context) throws Exception {
        Order order = context.getSeedData();
        //compute all totals for the order
        return context;

    }
}
```

Whereas this `Activity` would be defined in the `blCheckoutWorkflow`:

```java
public class CompleteOrderActivity extends BaseActivity<CheckoutContext> {

    @Override
    public CheckoutContext execute(CheckoutContext context) throws Exception {
        CheckoutSeed seed = context.getSeedData();

        seed.getOrder().setStatus(OrderStatus.SUBMITTED);
        seed.getOrder().setOrderNumber(new SimpleDateFormat("yyyyMMddHHmmssS").format(SystemTime.asDate()) + seed.getOrder().getId());
        seed.getOrder().setSubmitDate(Calendar.getInstance().getTime());

        return context;
    }

}
```

Composite activities also extend `BaseActivity`, so they are valid candidates for members of the activity list in the workflow definition. However, they differ in the fact that they accept a child workflow as their sole configuration. Whenever a composite activity is called by its parent workflow, that call is propagated down into the child workflows where its activities will be called in an orderly fashion. All child workflows are subject to the same ProcessContext instance and all exceptions are bubbled to the top and stopProcess calls at any level stop the entire nested workflow. By utilizing composite activities, we can achieve more complicated, nested behavior in our workflows.

## Error Handlers

```java
public interface ErrorHandler extends BeanNameAware {

    public void handleError(ProcessContext context, Throwable th) throws WorkflowException;

}
```

Error handlers are instances of the simple `ErrorHandler` interface (show above). The error handler provides the Broadleaf user an opportunity to perform some task when an exception takes place during the execution of a workflow. This could be as simple as logging the exception, or perhaps something more complicated like releasing resources. As stated earlier, all default Broadleaf workflows use the DefaultErrorHandler class, which simply logs the exception to `System.out` and bubbles the exception.

## Removing a Broadleaf Workflow
It is possible that you will want to remove one of the workflows that we have defined in the framework in order to perform more exotic functions or to handle very specific use cases. While we would not normally recommend this, we have provided an easy subclass for you to use, the `EmptySequenceProcess`. If you wanted to remove the `blCheckoutWorkflow` because you have subclassed `OrderService` and have your own implementation of `performCheckout()`, simply add this to your applicationContext:
```xml
<bean id="blCheckoutWorkflow" class="org.broadleafcommerce.core.workflow.EmptySequenceProcessor" />
```

## Provided Workflows
Below are some of the items that Broadleaf has workflow concepts for out of of the box ([full applicationContext definition](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/develop/core/broadleaf-framework/src/main/resources/bl-framework-applicationContext-workflow.xml)):

| Workflow Bean ID | Description
| :----------------- | :-----------------------
| [blAddItemWorkflow](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/master/core/broadleaf-framework/src/main/resources/bl-framework-applicationContext-workflow.xml#L92)  | Used when an item is added to the cart
| [blUpdateItemWorkflow](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/master/core/broadleaf-framework/src/main/resources/bl-framework-applicationContext-workflow.xml#L123) | Used when an item is updated in the cart
| [blRemoveItemWorkflow](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/master/core/broadleaf-framework/src/main/resources/bl-framework-applicationContext-workflow.xml#L174) | Used when an item is removed from the cart
| [blPricingWorkflow](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/master/core/broadleaf-framework/src/main/resources/bl-framework-applicationContext-workflow.xml#L52) | Used by `blPricingService` (which is used by OrderService) to price an Order
| [blCheckoutWorkflow](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/master/core/broadleaf-framework/src/main/resources/bl-framework-applicationContext-workflow.xml#L199) | Invoked by `blCheckoutService` in order to complete checkout for an Order (charge payments, decrement inventory, change status to SUBMITTED, etc)
