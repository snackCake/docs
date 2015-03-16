# Client Configuration for a Stand-Alone Solr Server

Once you have a stand-alone Solr server configured, you need to change the way that Broadleaf attempts to communicate with Solr.  First, you need to navigate to `site/src/main/webapp/WEB-INF/applicationContext.xml`.  In place of the following XML snippet:

```xml
<bean id="blSearchService" class="org.broadleafcommerce.core.search.service.solr.SolrSearchServiceImpl">
    <constructor-arg name="solrServer" ref="${solr.source.primary}" />
    <constructor-arg name="reindexServer" ref="${solr.source.reindex}" />
    <constructor-arg name="adminServer" ref="${solr.source.admin}" />
</bean> 
```

Add the following XML:

```xml
<bean id="solrServer" class="org.apache.solr.client.solrj.impl.LBHttpSolrServer">
        <constructor-arg value="${solr.url.primary}"/>
</bean>
<bean id="solrReindexServer" class="org.apache.solr.client.solrj.impl.LBHttpSolrServer">
        <constructor-arg value="${solr.url.reindex}"/>
</bean>
<bean id="solrAdminServer" class="org.apache.solr.client.solrj.impl.LBHttpSolrServer">
        <constructor-arg value="${solr.url.admin}"/>
</bean>
<bean id="blSearchService" class="org.broadleafcommerce.core.search.service.solr.SolrSearchServiceImpl">
    <constructor-arg name="solrServer" ref="${solr.source.primary}" />
    <constructor-arg name="reindexServer" ref="${solr.source.reindex}" />
    <constructor-arg name="adminServer" ref="${solr.source.admin}" />
</bean> 
```

See the `site/src/main/resources/runtime-properties/common.properties`. Comment out the lines that use `solrEmbedded` as the value for the various Solr sources.  Add the additional lines that assing `solrServer`, `solrReindexServer`, and `solrAdminServer` as the respective values for the `solr.source.*` property names.  These values will get replaced in the XML above, using property substitution.

```
#solr.source.primary=solrEmbedded
#solr.source.reindex=solrEmbedded
#solr.source.admin=solrEmbedded

# Comment out the solr.source.* above and use the following 
# if using non-embedded Solr
solr.source.primary=solrServer
solr.source.reindex=solrReindexServer
solr.source.admin=solrAdminServer
```

Next, for each relevant environment-specific properties file (e.g. `site/src/main/resources/runtime-properties/development.properties`) add the following:

```
solr.url.primary=http://localhost:8983/solr/primary
solr.url.reindex=http://localhost:8983/solr/reindex
solr.url.admin=http://localhost:8983/solr
```

Again, property substitution will be used to replace the `solr.url.*` properties with the correct values.  These values can be different in each environment.  For example, if you change `site/src/main/resources/runtime-properties/production.properties`, then the production URLs will be used in that environment. For example:

```
solr.url.primary=http://solr.prod:8983/solr/primary
solr.url.reindex=http://solr.prod:8983/solr/reindex
solr.url.admin=http://solr.prod:8983/solr
```

Alternatively, if you are using a Master / Slave configuration, or multiple Solr nodes, you can use a comma-delimited list of Solr servers.  The instance of the client that you are using is `org.apache.solr.client.solrj.impl.LBHttpSolrServer` which is a load balanced wrapper around `org.apache.solr.client.solrj.impl.HttpSolrServer`. For example:

```
solr.url.primary=http://slave1:8983/solr/primary,http://slave2:8983/solr/primary
solr.url.reindex=http://master:8983/solr/reindex
solr.url.admin=http://master:8983/solr
```

Broadleaf will now happily connect to your Stand-Alone Solr Server(s).


