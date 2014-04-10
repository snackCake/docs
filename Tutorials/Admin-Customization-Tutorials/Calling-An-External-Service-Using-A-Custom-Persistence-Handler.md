#Calling an External or Third Party Service using a Custom Persistence Handler

In this tutorial, we'll show you how to call an external or third party service whenever you need to update an entity in the admin.
For this example, let's say you would like to call a Third Party Address Validation service whenever you update an address on a Store entity. 
You can do this pretty easily using a custom persistence handler.

##Custom Persistence Handler

First, let's go ahead and create our Store Validation Persistence Handler class. It will look something like this:

```java
@Component("myStoreCustomPersistenceHandler")
public class StoreCustomPersistenceHandler extends CustomPersistenceHandlerAdapter {

    @Resource(name = "myAddressValidationService")
    protected MyAddressValidationService myAddressValidationService;
    
    @Override
    public Boolean canHandleUpdate(PersistencePackage persistencePackage) {
        ...
    }
    
    @Override
    public Entity update(PersistencePackage persistencePackage, DynamicEntityDao dynamicEntityDao, RecordHelper helper) throws ServiceException {
  	    ...
  	}

}
```
Notice that we've extended the CustomPersistenceHandlerAdapter class and we've implemented the `canHandleUpdate` and `update` methods.
We've also injected our own Third Party Address Validation service into the class so that we can validate the address.
For simplicity of this tutorial, we'll only call our custom Address Validation Service on an update of a store. 
In Admin usability terms, this means that this custom method would get called on the `Edit` screen for the entity you are trying to save. 

Since we only want our Persistence Handler to run for an `Update` on a Store entity, let's go ahead and implement the `canHandleUpdate` method.


```java
    @Override
    public Boolean canHandleUpdate(PersistencePackage persistencePackage) {
        String ceilingEntityFullyQualifiedClassname = persistencePackage.getCeilingEntityFullyQualifiedClassname();
        try {
            Class testClass = Class.forName(ceilingEntityFullyQualifiedClassname);
            return Store.class.isAssignableFrom(testClass);
        } catch (ClassNotFoundException e) {
            return false;
        }    
    }
```

Notice that in our method, we use the `ceilingEntityFullyQualifiedClassname` approach to determine whether or not the entity being passed to persist is a Store.


Now, we can go ahead and implement our `update` method to call our custom validation service.

```java
    @Override
    public Entity update(PersistencePackage persistencePackage, DynamicEntityDao dynamicEntityDao, RecordHelper helper) throws ServiceException {
        Entity entity = persistencePackage.getEntity();
        try {
            PersistencePerspective persistencePerspective = persistencePackage.getPersistencePerspective();
            Map<String, FieldMetadata> adminProperties = helper.getSimpleMergedProperties(Address.class.getName(), persistencePerspective);
            Object primaryKey = helper.getPrimaryKey(entity, adminProperties);
            Store adminInstance = (Store) dynamicEntityDao.retrieve(Class.forName(entity.getType()[0]), primaryKey);
            adminInstance = (Store) helper.createPopulatedInstance(adminInstance, entity, adminProperties, false);
            
            String address1 = entity.getPMap().get("address1");
            String address2 = entity.getPMap().get("address2");
            String city = entity.getPMap().get("city");
            String state = entity.getPMap().get("state");
            String zip = entity.getPMap().get("zip");
            String country = entity.getPMap().get("country");

            if (myAddressValidationService.validAddress(address1, address2, city, state, zip, country)) {
                adminInstance = dynamicEntityDao.merge(adminInstance);
                return helper.getRecord(adminProperties, adminInstance, null, null);
            }
            
            throw new IllegalArgumentException("Store Address is invalid.");

        } catch (Exception e) {
            LOG.error("Unable to update entity for " + entity.getType()[0], e);
            throw new ServiceException("Unable to update entity for " + entity.getType()[0], e);
        }
    }
```

If you look at the implementation for our `update` method you will notice a couple things. First, we will get the entity we are trying to persist (A Store) off the persistence package.
After this you will notice in the try block a couple lines of boiler plate admin code which retrieves a populated instance of the Store entity. In this example, I also demonstrate 
how you may also use the entity `PMap()` (Property Map) to retrieve custom dynamic fields you may have added to the entity from the `inspect` phase of the handler.
Either way, we now have access to the fields we want to validate and we can pass that to our custom service. Now, if our address validation returns true, we can go ahead and call
`dynamicEntityDao.merge()` and return our entity record using our utility method `helper.getRecord().

All put together our class would look like this:

```java
@Component("myStoreCustomPersistenceHandler")
public class StoreCustomPersistenceHandler extends CustomPersistenceHandlerAdapter {

    @Resource(name = "myAddressValidationService")
    protected MyAddressValidationService myAddressValidationService;
    
    @Override
    public Boolean canHandleUpdate(PersistencePackage persistencePackage) {
        String ceilingEntityFullyQualifiedClassname = persistencePackage.getCeilingEntityFullyQualifiedClassname();
        try {
            Class testClass = Class.forName(ceilingEntityFullyQualifiedClassname);
            return Store.class.isAssignableFrom(testClass);
        } catch (ClassNotFoundException e) {
            return false;
        }    
    }
    
    @Override
    public Entity update(PersistencePackage persistencePackage, DynamicEntityDao dynamicEntityDao, RecordHelper helper) throws ServiceException {
        Entity entity = persistencePackage.getEntity();
        try {
            PersistencePerspective persistencePerspective = persistencePackage.getPersistencePerspective();
            Map<String, FieldMetadata> adminProperties = helper.getSimpleMergedProperties(Address.class.getName(), persistencePerspective);
            Object primaryKey = helper.getPrimaryKey(entity, adminProperties);
            Store adminInstance = (Store) dynamicEntityDao.retrieve(Class.forName(entity.getType()[0]), primaryKey);
            adminInstance = (Store) helper.createPopulatedInstance(adminInstance, entity, adminProperties, false);
            
            String address1 = entity.getPMap().get("address1");
            String address2 = entity.getPMap().get("address2");
            String city = entity.getPMap().get("city");
            String state = entity.getPMap().get("state");
            String zip = entity.getPMap().get("zip");
            String country = entity.getPMap().get("country");

            if (myAddressValidationService.validAddress(address1, address2, city, state, zip, country)) {
                adminInstance = dynamicEntityDao.merge(adminInstance);
                return helper.getRecord(adminProperties, adminInstance, null, null);
            }
            
            throw new IllegalArgumentException("Store Address is invalid.");

        } catch (Exception e) {
            LOG.error("Unable to update entity for " + entity.getType()[0], e);
            throw new ServiceException("Unable to update entity for " + entity.getType()[0], e);
        }
    }

}

```

Finally, we will need to hook up our custom persistence handler so that it will run when any entity instance is persisted in the Admin.
We'll simply add it to the bean `blCustomPersistenceHandlers`

```xml
<bean id="blCustomPersistenceHandlers" class="org.springframework.beans.factory.config.ListFactoryBean" scope="prototype">
    <property name="sourceList">
        <list>
            <bean class="com.mycompany.admin.handler.StoreCustomPersistenceHandler" />
        </list>
    </property>
</bean>
```