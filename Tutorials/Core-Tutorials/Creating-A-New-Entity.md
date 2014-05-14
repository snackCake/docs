# Creating a New Custom Entity

### Overview
This tutorial explains how you can add a new custom entity to a custom Broadleaf application.    

For this example, we will create a simple Account entity with properties for name and account number.

This tutorial will serve as the basis for other tutorials including one that shows you how to add a new section and edit screen to the Admin.

### Interface
Let's start with an interface we want to implement.   

```java
package com.mycompany.tutorial;


public interface Account {

    Long getId();

    void setId(Long id);

    String getName();

    void setName(String name);

    String getAccountNumber();

    void setAccountNumber(String accountNumber);
}
```

### Implementation
Next, let's add a bare-bones JPA implementation for this class.

#### Step One - Implement The Class
Below is a quick example that implements the new entity.   This is really just JPA.

```java
package com.mycompany.tutorial;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "ACCOUNT")
public class AccountImpl implements Account {

    @Id
    @Column(name = "ACCOUNT_ID")
    protected Long id;

    @Column(name = "NAME")
    protected String name;

    @Column(name = "ACCOUNT_NBR")
    protected String accountNumber;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAccountNumber() {
        return accountNumber;
    }

    public void setAccountNumber(String accountNumber) {
        this.accountNumber = accountNumber;
    }
}
```


#### Step Two Register the Class in persistence.xml
The new entity needs to be registered with your Broadleaf application's persistence.xml.   

The Broadleaf DemoSite manages this file in the core project.   The following shows a single line edit to the file located in '/core/src/main/resources/META-INF/persistence-core.xml' in the DemoSite core project.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://java.sun.com/xml/ns/persistence"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd"
             version="2.0">
             
    <persistence-unit name="blPU" transaction-type="RESOURCE_LOCAL">
        <non-jta-data-source>jdbc/web</non-jta-data-source>

        <!-- Added this line -->
        <class>com.mycompany.tutorial.AccountImpl</class>
        
        <exclude-unlisted-classes/>
    </persistence-unit>
</persistence>
```


#### Step Three Add an ID Generator
For most (if not all) entities in a Broadleaf application, you'll want to use an ID generator.    The ID Generator provides an efficient way to generate ids and is required for entities to be manged in the Broadleaf Admin.

To add, this make the following change to the .java file from Step 1. 

```java
    @Id
    @GeneratedValue(generator= "AccountId")
    @GenericGenerator(
        name="AccountId",
        strategy="org.broadleafcommerce.common.persistence.IdOverrideTableGenerator",
        parameters = {
            @Parameter(name="segment_value", value="AccountImpl"),
            @Parameter(name="entity_name", value="com.mycompany.tutorial.AccountImpl")
        }
    )    
    @Column(name = "ACCOUNT_ID")
    protected Long id;
```

### Testing
At this point, all we have is a JPA domain, but you can test by building and running your DemoSite and then checking to see if the Account table was created in your database. 

As long as you are compiling and starting ok, you can move on to the - [[Admin Customization | Admin Customization Tutorials]] dealing with creating new admin sections.