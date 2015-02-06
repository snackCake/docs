# Embedded Solr

Embedded Solr is the default deployment option with Broadleaf.  This is generally for simplicity.  Embedded Solr is a mechanism that allows for Solr to be run as a service inside Broadleaf, where the index is written to the local file system, usually a temp directory.  When you download and start Broadleaf's Demo Site, this is the flavor of Solr that is used.  This configuration works great for getting started, and for development environments.  However, this is not usually the recommended configuration for environments like production.

## How it works

When the Site (customer facing site as opposed to the Admin) is started, one of the Spring configurations is the Broadleaf SearchService, with the Spring bean name _blSearchService_.  The default implementation is ```org.broadleafcommerce.core.search.service.solr.SolrSearchServiceImpl```.  Besides being responsible for issuing requests to Solr, this class is responsible for the basic setup of Solr-related dependencies.  It has several constructors:

```java
public SolrSearchServiceImpl(String solrServer) throws IOException, ParserConfigurationException, SAXException {
   ...
}
```

It also has several other constructors that accept Strings, but that delegate back to the above constructor, for consistency:

```java
public SolrSearchServiceImpl(String solrServer, String reindexServer)
    throws IOException, ParserConfigurationException, SAXException {
    this(solrServer);
}
```

```java
public SolrSearchServiceImpl(String solrServer, String reindexServer, String adminServer)
    throws IOException, ParserConfigurationException, SAXException {
    this(solrServer);
}
```

These constructors activate the embedded Solr Server.  The details can be found by looking at the single argument constuctor, above.  However, there are a few things to understand about this. First, if the String "solrhome" is passed to the constructor, then then a directory is created in the temp directory, defined by ```java.io.tmpdir```, which is usually the operating system's temporary directory.  If any values except "solrhome" is passed to the constructor, Broadleaf assumes that it is the directory that you wish to use to store the configuration and index data.  The system will create the appropriate subdirectories and put the appropriate files in them to run an embedded Solr instance.  By default, two Solr cores are created, "primary" and "reindex".  This allows Broadleaf to reindex the catalog in a background thread to an "offline" core, while allowing the "online" core to continue to serve requests.  When indexing is complete, Broadleaf swaps the cores, making the newly reindex core primary and the former primary core comes offline to be used next time reindexing occurs.

Broadleaf is configured, out of the box, to use embedded Solr.  Here is how the configuration works: In the site/src/main/webapp/WEB-INF directory, there is a file called applicationContext.xml.  This contains the Solr configuration for Site (remember that Admin does not use Solr by default in the Community Edition).  There is a Spring Bean, defined as "solrEmbedded", which is simply a String:

```xml
<bean id="solrEmbedded" class="java.lang.String">
    <constructor-arg value="solrhome"/>
</bean>
```

As you can see, the value of this String is "solrhome".  There is also a Bean called "blSearchService".  This defines the actual search service, which must implement the interface ```org.broadleafcommerce.core.search.service.SearchService```.  Here is the default configuration:

```xml
<bean id="blSearchService" class="org.broadleafcommerce.core.search.service.solr.SolrSearchServiceImpl">
    <constructor-arg name="solrServer" ref="${solr.source.primary}" />
    <constructor-arg name="reindexServer" ref="${solr.source.reindex}" />
    <constructor-arg name="adminServer" ref="${solr.source.admin}" />
</bean>
```

Notice the property substitutions.  You can replace the property substitutions with a concrete Spring Bean name.  For simplicity, we store the bean names in properties files so you can use different implementations in different environments.  In site/src/main/resources/runtime-properties/common.properties, you'll notice the settings:

```
solr.source.primary=solrEmbedded
solr.source.reindex=solrEmbedded
solr.source.admin=solrEmbedded
```

This cause Spring to replace the property placeholders with the Spring Bean, solrEmbedded, whos value is "solrhome".  As described, above, this causes the constructor which accepts a single String to be invoked, which starts Solr in embedded mode using a temp director as a local data store.



