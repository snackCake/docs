# Customizing Payment

In this tutorial, we will walk through adding a new payment type to support employee payroll deduction as a method of payment. This tutorial only applies if you would like to create your own type of OrderPayment or one that doesn't exist in the framework. If you would like to customize payment to integrate with a Payment Provider please read our [[Payment]] and [[Creating a Payment Gateway Module]] documentation.

## Prerequisites
- [[Service Extension | Extending Services]]
- [[Entity Extension | Extending Entities]]

## OrderPayment

First, we will need to support a new Referenced implementation if we plan on storing account information for this payment type and:

- the information must be kept secure
- or, the information required to represent the account is more complicated than a simple account number

Please read the [[Payment Security and PCI Compliance]] section for more information on ensuring data is secure.

For this example, we will assume that employee account information passes this litmus test for creating a Referenced implementation. First, we'll create a custom interface that extends Referenced and defines our additional custom fields.

```java
public interface EmployeeOrderPayment extends Referenced {

    public String getEmployeeId();

    public void setEmployeeId(String employeeId);

    public String getPin();

    public void setPin(String pin);

}
```

Next, we need to create our implementation of `EmployeeOrderPayment`, which yields a new entity (since this is persisted information).

```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
@Table(name = "MY_EMPLOYEE_PAYMENT")
public class EmployeeOrderPaymentImpl implements EmployeeOrderPayment {

    private static final long serialVersionUID = 1L;

    @Transient
    protected EncryptionModule encryptionModule;

    @Id
    @GeneratedValue(generator = "EmployeePaymentId", strategy = GenerationType.TABLE)
    @TableGenerator(name = "EmployeePaymentId", table = "SEQUENCE_GENERATOR", pkColumnName = "ID_NAME", valueColumnName = "ID_VAL", pkColumnValue = "EmployeePaymentInfoImpl", allocationSize = 50)
    @Column(name = "PAYMENT_ID")
    protected Long id;

    @Column(name = "REFERENCE_NUMBER", nullable=false)
    @Index(name="EMPLOYEE_INDEX", columnNames={"REFERENCE_NUMBER"})
    protected String referenceNumber;

    @Column(name = "PIN", nullable=false)
    protected String pin;

    @Column(name = "EMPLOYEE_ID", nullable=false)
    protected String employeeId;


    public String getPin() {
        return encryptionModule.decrypt(pin);
    }

    public void setPin(String pin) {
        this.pin = encryptionModule.encrypt(pin);
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getReferenceNumber() {
        return referenceNumber;
    }

    public void setReferenceNumber(String referenceNumber) {
        this.referenceNumber = referenceNumber;
    }

    public String getPin() {
        return encryptionModule.decrypt(pin);
    }

    public void setPin(String pin) {
        this.pin = encryptionModule.encrypt(pin);
    }

    public String getEmployeeId() {
        return employeeId;
    }

    public void setEmployeeId(String employeeId) {
        this. employeeId = employeeId;
    }

    public EncryptionModule getEncryptionModule() {
        return encryptionModule;
    }

    public void setEncryptionModule(EncryptionModule encryptionModule) {
        this.encryptionModule = encryptionModule;
    }

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + ((employeeId == null) ? 0 : employeeId.hashCode());
        result = prime * result + ((pin == null) ? 0 : pin.hashCode());
        result = prime * result + ((referenceNumber == null) ? 0 : referenceNumber.hashCode());
        return result;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj)
            return true;
        if (obj == null)
            return false;
        if (getClass() != obj.getClass())
            return false;
        EmployeePaymentInfoImpl other = (EmployeePaymentInfoImpl) obj;

        if (id != null && other.id != null) {
            return id.equals(other.id);
        }

        if (employeeId == null) {
            if (other. employeeId h != null)
                return false;
        } else if (! employeeId.equals(other.employeeId))
            return false;
        if (pin == null) {
            if (other.pin != null)
                return false;
        } else if (!pin.equals(other.pin))
            return false;
        return true;
    }

}
```

Of course, a new table in the secure database (see [[Data and Service Models]]) entitled `MY_EMPLOYEE_PAYMENT` should be available to back this entity. The `id` and `referenceNumber` fields are managed by Broadleaf Commerce. The `pin` and `employeeId` fields are your custom fields and we'll talk more about where you manage them later. The `encryptionModule` field is discussed in the "Securing Account Information" section later. The next step is to create a new PaymentType to support the employee payment type.

```java
public class EmployeePaymentType extends PaymentType {

    public static final PaymentType EMPLOYEE = new PaymentType("EMPLOYEE", "Employee");

}
```

Your payment type must extend PaymentType so that Broadleaf Commerce can properly recognize it as a valid payment type. Here, we've simply added one new PaymentType of "EMPLOYEE". Next you'll need to implement several Payment Gateway interfaces so that the checkout workflow can confirm and capture those amounts. For example one of the important classes you will create must implement the `PaymentGatewayTransactionService` to capture those amounts. Most likely in this scenario the funds will be captured in the checkout workflow in the `ValidateAndConfirmPaymentActivity`. So you will also need to implement the 
`PaymentGatewayTransactionConfirmationService` which will most likely call the capture method implementation that you created above.

See our [[Creating a Payment Gateway Module]] documentation for more details on the plumbing of wiring up the Payment Gateway services you created.


```java
public class EmployeeTransactionServiceImpl implements PaymentGatewayTransactionService {

    @Override
    public PaymentResponseDTO capture(PaymentRequestDTO paymentRequestDTO) throws PaymentException {
        //TODO Do something to charge the employee for the purchase.
        return null;
    }

}
```

```java
public class EmployeeTransactionConfirmationServiceImpl implements PaymentGatewayTransactionConfirmationService {

    @Override
    public PaymentResponseDTO confirmTransaction(PaymentRequestDTO paymentRequestDTO) throws PaymentException {
        //TODO call your EmployeeTransactionServiceImpl.capture() to confirm the transaction
        return null;
    }

}
```

For employee purchase, we only care about supporting the capture method of the PaymentGatewayTransactionService. In your capture implementation, perform whatever tasks are necessary to charge the employee for the purchase. This could be as simple as making a database entry which a separate batch process reads at a later time to perform a payroll deduction or making a web service call of some sort. Whatever fits your circumstance - the choice is yours.

Now, during checkout, Broadleaf Commerce will use your new gateway implementation for any OrderPayment instances associated with the order whose PaymentType is "EMPLOYEE". Note, while the new payment service is included in the regular payment workflow used by Broadleaf Commerce during checkout, this service is directly available to all of your code like any other Broadleaf Commerce service. Simply inject the service into your own beans to acquire payment transaction functionality outside of the checkout workflow.

## Managing Custom Payment Type Data

Recall the earlier discussion regarding the custom employee payment type. Remember that we utilized a custom Referenced implementation. To be able to persist this employee account information, we'll need to add support to the SecureOrderPaymentService and SecureOrderPaymentDao. The first thing to do is extend the SecureOrderPaymentDao interface to support our custom payment.

```java
public interface MySecureOrderPaymentDao extends SecureOrderPaymentDao {

    public EmployeeOrderPayment findEmployeeInfo(String referenceNumber);

    public EmployeeOrderPayment createEmployeePaymentInfo();

}
```

Now, let's extend the `SecureOrderPaymentDaoImpl` to implement the find and create functionality for our custom payment.

```
public class MySecureOrderPaymentDaoImpl extends SecureOrderPaymentDaoImpl implements MySecureOrderPaymentDao {

    public EmployeeOrderPayment createEmployeeOrderPayment() {
    EmployeeOrderPayment response = (EmployeeOrderPayment) entityConfiguration.createEntityInstance("com.mycompany.payment.domain.EmployeeOrderPayment");
        response.setEncryptionModule(encryptionModule);
        return response;
    }

    @SuppressWarnings("unchecked")
    public EmployeeOrderPayment findEmployeeInfo(String referenceNumber) {
        Query query = em.createQuery("SELECT employeePayment FROM com.mycompany.payment.domain.EmployeeOrderPayment employeePayment WHERE employeePayment.referenceNumber = :referenceNumber");
        query.setParameter("referenceNumber", referenceNumber);
        List<EmployeeOrderPayment> infos = query.getResultList();
        EmployeeOrderPayment response = (infos == null || infos.size() == 0) ? null : infos.get(0);
        if (response != null) {
            response.setEncryptionModule(encryptionModule);
        }
        return response;
    }

}
```

For the `createEmployeeOrderPayment` method, we use the EntityConfiguration instance to create an instance of our entity. Then when add the encryption module supplied by the superclass. To be able to use the EntityConfiguration instance, we must have configuration supplied for the entity in our application context. Here's an example to support our EmployeeOrderPayment entity.

```xml
<bean id="com.mycompany.payment.domain.EmployeeOrderPayment" class="com.mycompany.payment.domain.EmployeeOrderPaymentImpl" scope="prototype"/>
```

We also need to notify Broadleaf Commerce to use our custom dao, instead of its default, internal one. We accomplish this by setting an instance of our dao to the key id `blSecurePaymentInfoDao` in our application context.

```xml
<bean id="blSecureOrderPaymentDao" class="com.mycompany.payment.dao.MySecureOrderPaymentDaoImpl"/>
```

Next, let's extend the SecureOrderPaymentServiceImpl to support our custom payment.

```java
public class MySecureOrderPaymentServiceImpl extends SecureOrderPaymentServiceImpl {

    @Override
    public Referenced create(PaymentType paymentType) {
        if (EmployeePaymentType.EMPLOYEE.equals(paymentType)) {
            return ((MySecureOrderPaymentDao) secureOrderPaymentDao).createEmployeeOrderPayment();
        }
        return super.create(paymentType);
    }

    @Override
    public Referenced findSecureOrderPayment(String referenceNumber, PaymentType paymentType) throws WorkflowException {
        if (EmployeePaymentType.EMPLOYEE.equals(paymentType)) {
            return ((MySecureOrderPaymentDao) secureOrderPaymentDao).findEmployeeInfo(referenceNumber);
        }
        return super.findSecureOrderPayment(referenceNumber, paymentType);
    }

}
```

Finally, we need to add configuration to our application context to notify Broadleaf Commerce to use our custom secure payment service implementation instead of its default internal one. We accomplish this by assigning the key id "blSecurePaymentInfoService" to an instance of our custom secure payment service.

```xml
<bean id="blSecureOrderPaymentService" class="com.mycompany.payment.service.MySecureOrderPaymentServiceImpl"/>
```
