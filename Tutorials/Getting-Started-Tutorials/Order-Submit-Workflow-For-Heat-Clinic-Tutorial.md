# Order Submit Workflow For Heat Clinic Tutorial

> **NOTE:** This tutorial requires you to complete the [[Extending Customer For Heat Clinic Tutorial]] first. 

## Hooking into the submit order workflow

First, let's look at the default checkout workflow provided by broadleaf:

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
    <bean p:order="6000" id="blCompleteOrderActivity" class="org.broadleafcommerce.core.checkout.service.workflow.CompleteOrderActivity">
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
                <ref bean="blCompleteOrderActivity" />
            </list>
        </property>
        <property name="defaultErrorHandler" ref="blDefaultErrorHandler"/>
    </bean>
```

> This is a copy of the default workflow. You can find the default workflows in the following place in the Broadleaf codebase: `core/broadleaf-framework/src/main/resources/bl-framework-applicationContext-workflow.xml`

## Create a new activity

We need to add a new activity to record the average heat level for a given customer. Let's create that activity. In the `site` project, we'll create `RecordHeatRangeActivity.java` in the `com.mycompany.checkout.service.workflow` package with the following contents:

```java
public class RecordHeatRangeActivity extends BaseActivity {
    private static final Log LOG = LogFactory.getLog(RecordHeatRangeActivity.class);
    
    @Resource(name = "blCustomerService")
    protected CustomerService customerService;

    @Override
    public ProcessContext execute(ProcessContext context) throws Exception {
        CheckoutSeed seed = (CheckoutSeed) context.getSeedData();
        Order order = seed.getOrder();
        int orderTotalHeatRating = 0;
        int productCount = 0;
        
        for (DiscreteOrderItem doi : order.getDiscreteOrderItems()) {
            ProductAttribute heatRating = doi.getProduct().getProductAttributes().get("heatRange");
            try {
                orderTotalHeatRating += (doi.getQuantity() * Integer.parseInt(heatRating.getValue()));
                productCount += doi.getQuantity();
            } catch (Exception e) {
                LOG.warn("Unable to parse the heat range. Product id: " + doi.getProduct().getId());
            }
        }
        
        HCCustomer customer = (HCCustomer) CustomerState.getCustomer();
        customer.setNumSaucesBought(customer.getNumSaucesBought() + productCount);
        customer.setTotalHeatRating(customer.getTotalHeatRating() + orderTotalHeatRating);
        customer = (HCCustomer) customerService.saveCustomer(customer);
        CustomerState.setCustomer(customer);
        
        return context;
    }
}
```

> Note: You should choose `org.broadleafcommerce.core.order.domain.Order`, `org.apache.common.logging.Log`, and `org.apache.common.logging.LogFactory` as imports.

Notice that the activity loops through all current products in the order and assigns the appropriate heat range to the customer.

## Add our activity to the workflow

We want to add our new activity between the `RecordOfferUsageActivity` and the `CompleteOrderActivity`.
In this example, within `applicationContext.xml`, we override the blCheckoutWorkflow and set the `p:order="5500"` in the bean declaration, like so:

```xml
    <bean id="blCheckoutWorkflow" class="org.broadleafcommerce.core.workflow.SequenceProcessor">
        <property name="activities">
            <list>
                <bean p:order="5500" class="com.mycompany.checkout.service.workflow.RecordHeatRangeActivity"/>
            </list>
        </property>
    </bean>
```

and our checkout workflow is complete. Check out the end result after checking out an order that had (1) level 6 sauce and (1) level 8 sauce:

![Averaged Heat Range](submit-order-workflow-tutorial-1.png)
