# Custom Pricing Activities

Sometimes it is desirable to add in a custom pricing activity. For example, you may have a scenario where a particular type of order will incur a surcharge, and you would like to support the application of this surcharge in the pricing workflow. Let's take some time to look at this scenario more in-depth and develop an example. Of course, there are many reasons why someone would want to add a custom pricing activity, and this example should give you the tools and information necessary to develop a solution to any pricing problem.

First, we need to create a place to hold our custom surcharge. The most likely place to persist this value would be to a new field on a customized extension of the Order entity (see the [[Entity Extension Guide|Extending Entities]]). Here's an example extension of the Broadleaf Commerce Order:

```java
@Entity
@Table(name = "MY_ORDER")
public class MyOrderImpl extends OrderImpl {
    private static final long serialVersionUID = 1L;

    @Column(name = "ORDER_SURCHARGE")
    protected BigDecimal surcharge;

    public Money getSurcharge() {
       return surcharge == null ? null : new Money(surcharge);
    }

    public void setSurcharge(Money surcharge) {
       this.surcharge = Money.toAmount(surcharge);
    }
}
```

In compliance with Broadleaf Commerce requirements for entity extension, you will also need to register the new entity with Order key id in your application context.

```xml
<bean id="org.broadleafcommerce.core.order.domain.Order" class="com.mycompany.order.domain.MyOrderImpl" scope="prototype"/>
```

Now, all instances of Order created and used by Broadleaf Commerce will be instances of MyOrderImpl. Of course, you will need to make sure you add a `MY_ORDER` table to the non-secure schema (see [[Database Model]]) that contains the `ORDER_ID` and `ORDER_SURCHARGE` fields.

With the entity in place, we can proceed with creating our custom pricing activity implementation.

```java
public class SurchargeActivity extends BaseActivity<ProcessContext<Order>> {

    public ProcessContext<Order> execute(ProcessContext<Order> context) throws Exception {
        MyOrderImpl order = (MyOrderImpl) context.getSeedData();
        if (order.getSubTotal().lessThan(new BigDecimal("10"))) {
            order.setSurcharge(new Money("20"));
        }

        context.setSeedData(order);
        return context;
    }

}
```

This simple pricing activity casts the ProcessContext seed data to our MyOrderImpl class so that we can access the surcharge field. In this example, we add a surcharge if the order subtotal is less than a threshold value.

Now that you're supporting the surcharge, you'll probably also want to include the surcharge in the order total calculation. To accomplish this, we'll need to override the `OrderTotalActivity`.

```java
public class SurchargeTotalActivity extends TotalActivity<ProcessContext<Order>> {

    @Override
    public ProcessContext<Order> execute(ProcessContext<Order> context) throws Exception {
        context = super.execute(context);
        MyOrderImpl order = (MyOrderImpl) context.getSeedData();
        if (order.getSurcharge() != null) {
            Money total = order.getTotal();
            order.setTotal(total.add(order.getSurcharge()));
        }

        context.setSeedData(order);
        return context;
    }

}
```

Here, we first call the parent execute method so that the order total can be calculated as normal. Then, we add in an additional step to reset the order total by adding in our surcharge value, if applicable.

With the logic in place to handle the new calculation, we need to override the `blTotalActivity` bean with our new `TotalActivity` as well as add in our `SurchargeActivity`. Since the `TotalActivity` is in position **8000** in the pricing workflow, this looks like the following inside of your applicationContext:

```xml
<!-- Override the blTotalActivity with our custom totaling activity -->
<bean p:order="8000" id="blTotalActivity" class="com.mycompany.core.workflow.SurchargeTotalActivity" />
<!-- Add our new pricing activity that should occur immediately before the total activity -->
<bean id="blPricingWorkflow" class="org.broadleafcommerce.core.workflow.SequenceProcessor">
    <property name="activities">
        <list>
            <bean p:order="7999" class="com.mycompany.core.workflow.SurchargeActivity" />
        </list>
    </property>
</bean>
```

And that's it! Now you should have surcharges included in all of your order totals.
