# Adding JavaScript Tutorial

When customizing the admin application, it is often desirable to add JavaScript files to the application. 

## Set up a resource location for JS files

First, we must hook up a new resource location for our JS files. We can do this by adding the following to `applicationContext-admin.xml`:

```xml
<bean id="jsLocations" class="org.springframework.beans.factory.config.ListFactoryBean">
    <property name="sourceList">
        <list>
            <value>/js/</value>
        </list>
    </property>
</bean>
<bean class="org.broadleafcommerce.common.extensibility.context.merge.LateStageMergeBeanPostProcessor">
    <property name="collectionRef" value="jsLocations" />
    <property name="targetRef" value="blJsLocations" />
</bean>
```

This location will serve files that live in `admin/src/main/webapp/js`, so that's where you would want to place your files. Of course, you are certainly able to configure a different location if you prefer.

## Add JS files to the out of box bundles

After that, we want to attach our JS files to the appropriate out of box bundle in the admin application. This will allow us to not have to override footer.html, which will help prevent future effort when upgrading Broadleaf.

We can attach our files with the following XML, also in `applicationContext-admin.xml`:

```xml
<bean id="blCustomAdminJsFileList" class="org.springframework.beans.factory.config.ListFactoryBean" >
    <property name="sourceList">
        <list>
            <value>admin/custom-product.js</value>
        </list>
    </property>
</bean>
<bean class="org.broadleafcommerce.common.extensibility.context.merge.LateStageMergeBeanPostProcessor">
    <property name="collectionRef" value="blCustomAdminJsFileList" />
    <property name="targetRef" value="blJsFileList" />
</bean>
```

This snippet will load a file at `admin/src/main/webapp/js/admin/custom-product.js` after all of the Broadleaf admin JavaScript, but before any field initialization happens.


