# Row-level Security

> Note: This is new in 3.1.2-GA

The admin security out of the box only protects against specific classes being managed in the admin. For instance, you might only allow CSRs to modify Orders but do not what them seeing or modifying Products. However, there are scenarios where you only want certain admin users to modify *certain* Products or *certain* categories. With row-level security you can restrict the following actions in the admin:

- Display (fetch)
- Update
- Remove

This is achieved through the ^[javadoc:org/broadleafcommerce/openadmin/server/security/service/RowLevelSecurityService]` interface:

```java
public interface RowLevelSecurityService {

    public void addFetchRestrictions(AdminUser user, String ceilingEntity, List<Predicate> restrictions, List<Order> sorts,
            Root entityRoot,
            CriteriaQuery criteria,
            CriteriaBuilder criteriaBuilder);

    public boolean canUpdate(AdminUser user, Entity entity);

    public boolean canRemove(AdminUser user, Entity entity);

    public GlobalValidationResult validateUpdateRequest(AdminUser user, Entity entity, PersistencePackage persistencePackage);

    public GlobalValidationResult validateRemoveRequest(AdminUser user, Entity entity, PersistencePackage persistencePackage);
}
```

This service also has a list of `RowLevelSecurityProvider`s (which is a marker interface that extends from this) so that multiple components can define different security constraints.

## Restricting Fetch

One of the ways to provide security is on fetch requests. This allows you to restrict the result set that is returned by various admin operations. Additional `Predicate`s should be added to the **restrictions** list rather than aattached to the given **criteria** directly.

Suppose for instance that you have a custom extension of Product that had a Store property. You had also extended an `AdminUser` to have a store that they owned, and you wanted to ensure that they could only see products for the store that they owned. Your entities might look like this:

```java
@Entity
public class MyAdminUser extends AdminUserImpl {

    @ManyToOne(targetEntity = StoreImpl.class)
    protected Store store;
}
```

```java
@Entity
public class MyProduct extends ProductImpl {
    
    @ManyToOne(targetEntity = StoreImpl.class)
    protected Store store;

    public Store getStore() {
        return store;
    }
}
```

And now you would override the `addFetchRestrictions` method to restrict the products shown for the current admin user:

```java
@Component
public class ProductStoreRowSecurityProvider {

   public void addFetchRestrictions(AdminUser currentUser, String ceilingEntity, List<Predicate> restrictions, List<Order> sorts,
            Root entityRoot,
            CriteriaQuery criteria,
            CriteriaBuilder criteriaBuilder) {
        if (Product.class.isAssignableFrom(Class.forName(ceilingEntity)) {
            Store adminStore = ((MyAdminuser) currentUser).getStore();
        
            Predicate storeRestriction = criteriaBuilder.equal(root.get("store"), adminStore);
            
            restrictions.add(storeRestriction);
        }
    } 
}
```

and then hook it up to the list of providers in the system:

```xml
<bean id="blCustomRowSecurityProviders" class="org.springframework.beans.factory.config.ListFactoryBean" >
     <property name="sourceList">
        <list>
            <ref bean="productStoreRowSecurityProvider" />
        </list>
    </property>
</bean>
<bean class="org.broadleafcommerce.common.extensibility.context.merge.LateStageMergeBeanPostProcessor">
    <property name="collectionRef" value="blCustomRowSecurityProviders" />
    <property name="targetRef" value="blRowLevelSecurityProviders" />
</bean>
```

> During debugging it might be a bit confusing that your `addFetchRestrictions` is being hit twice for every request. This is so that your criteria can be added to *count* queries as well as *select* queries

## `canUpdate` and `canRemove` vs `validateUpdate` and `validateRemove`

The "can" methods deal with button removal in the admin panel while the validation methods have to do with the user actually POSTing a request to the server. Using the above example, you might want admin users to see all of the PRoducts in the system but only edit Products that match their Store. In this case if you were to return 'false' from the 'canUpdate' method, when the user went to view the details of a Product then the save button would not be shown and also the entire form would be in a readonly state. Returning false from `canRemove` will hide the `Remove` button that shows up on forms.

> If a user cannot update a record this does not necessarily mean that they also cannot remove a record. You will need to return false from both.

If you are properly returning false from the `canUpdate` and `canRemove` methods, then it is unlikely that the `validateUpdate` and `validateRemove` methods will be invoked. However, this is still possible if the admin user either manually submits the POST request (by modifying the UI layer in their browser to show the button or sending a separate request) or if an update should be prevented only on certain values in the form. In this case, you have the opportunity to provide messaging to the user via the error result to prevent the update and/or remove.
