# Client Setup

Once Zookeeper and Solr have been installed and configured, you can instruct Broadleaf to begin using them.  This is a simple matter of configuration.  Assuming you are using Broadleaf's Demo Site distribution, or some derivitive, you can find the Solr configuration in the ```site/src/main/webapp/WEB-INF/applicationContext.xml```.  In particular, you will want to find the section of the config that defines the ```blSearchService``` bean:

```xml
<!--     <bean id="solrServer" class="org.apache.solr.client.solrj.impl.HttpSolrServer"> -->
<!--         <constructor-arg value="${solr.url.primary}"/> -->
<!--     </bean> -->
<!--     <bean id="solrReindexServer" class="org.apache.solr.client.solrj.impl.HttpSolrServer"> -->
<!--         <constructor-arg value="${solr.url.reindex}"/> -->
<!--     </bean> -->
<!--     <bean id="solrAdminServer" class="org.apache.solr.client.solrj.impl.HttpSolrServer"> -->
<!--         <constructor-arg value="${solr.url.admin}"/> -->
<!--     </bean> -->
<bean id="blSearchService" class="org.broadleafcommerce.core.search.service.solr.SolrSearchServiceImpl">
    <constructor-arg name="solrServer" ref="${solr.source.primary}" />
    <constructor-arg name="reindexServer" ref="${solr.source.reindex}" />
    <constructor-arg name="adminServer" ref="${solr.source.admin}" />
</bean>
```

The above default configuration needs to change to the following:

```xml
<bean id="solrServer" class="org.apache.solr.client.solrj.impl.CloudSolrServer">
    <constructor-arg value="${solr.url.primary}"/>
    <property name="defaultCollection" value="primary"/>
</bean>
<bean id="solrReindexServer" class="org.apache.solr.client.solrj.impl.CloudSolrServer">
    <constructor-arg value="${solr.url.reindex}"/>
    <property name="defaultCollection" value="reindex"/>
    <property name="requestWriter">
        <bean class="org.apache.solr.client.solrj.request.RequestWriter"/>
    </property>
</bean>
<!-- We still don't need the admin server!
<bean id="solrAdminServer" class="org.apache.solr.client.solrj.impl.HttpSolrServer">
    <constructor-arg value="${solr.url.admin}"/>
</bean-->
<bean id="blSearchService" class="org.broadleafcommerce.core.search.service.solr.SolrSearchServiceImpl">
    <constructor-arg name="solrServer" ref="${solr.source.primary}" />
    <constructor-arg name="reindexServer" ref="${solr.source.reindex}" />
</bean>
```

In ```site/src/main/resources/common.properties``` you'll want to make sure you have the following configuration:

```
...
solr.source.primary=solrServer
solr.source.reindex=solrReindexServer

solr.url.primary=localhost:2181,localhost:2182,localhost:2183
solr.url.reindex=localhost:2181,localhost:2182,localhost:2183

...
```

And There are a few important things to note about the above configuration.  First, notice that the implementation of the SolrServer instances (SolrJ client) is ```org.apache.solr.client.solrj.impl.CloudSolrServer```.  Also, notice that the primary and reindex instances have different defaultCollection values, matching the aliases discussed in the section on setting up SolrCloud. Notice that the reindex instance has a ```requestWriter``` defined.  The reason for this is that the default request writer is an instance of ```org.apache.solr.client.solrj.impl.BinaryRequestWriter```.  This does not properly serialize BigDecimal (or BigInteger) values, which Broadleaf uses extensively.  The above configuration is an XML request writer that handles those values just fine.  The values in the properties file configuration indicate to Spring to substibute the values for the bean names and the URL values.  These can be changed on a per environment basis.

Finally, there are a few additional advanced configurations.  If you have not already configured the appropriate SolrCloud collections and aliases, as described in the section on  [[Solr Cloud Setup]], do not worry.  Broadleaf will create the necessary collections and aliases.  However, there are a few pieces of data that are required to do this and Broadleaf uses some basic defaults.  When creating a collection, Broadleaf needs to know how many shards to create.  The default is 2.  This value can be changed by overriding a property by adding it to the ```site/src/main/resources/common.properties```:

```
solr.cloud.defaultNumShards=3
```

You cannot have more shards than you have Solr instances.

Additionally, in the section on [[Solr Cloud Setup]], we talked about how we upload a Solr configuration directory using the configuration name "blc".  If you chose to use a different name, and you want Broadleaf to configure your collections and aliases, you can override that property as well in the ```site/src/main/resources/common.properties```:

```
solr.cloud.configName=someOtherConfigName
```

When you build and start Broadleaf now, it will use the Solr configuration above to connect to Zookeeper to get the cluster information, and then it will load balance search requests appropriately among cluster members against the "primary" alias.  If you are using the ```blSolrIndexService```, an instance of ```org.broadleafcommerce.core.search.service.solr.SolrIndexServiceImpl```, Broadleaf will reindex against the "reindex" alias.  When it finishes reindexing, it will reassign the aliases so that the collection that was reindexed will become the primary, and vice versa.

Happy Searching!


