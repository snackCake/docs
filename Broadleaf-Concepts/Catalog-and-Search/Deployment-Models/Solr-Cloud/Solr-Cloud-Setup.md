# SolrCloud Setup

Assuming you have Zookeeper installed and started, you can install and configure Solr.  For additional information on SolrCloud setup, see the [SolrCloud documentation](https://cwiki.apache.org/confluence/display/solr/SolrCloud).

This documentation assumes you are using Solr 4.10.3. Download Solr [here](http://lucene.apache.org/solr/downloads.html) and unzip it to the local file system.  We'll call this location SOLR_HOME.  Solr provides a number of examples and features out of the box.  To simplify things and get to lean setup procedure, follow these steps:

- Create an empty directory on the file system wherever you like called `blcSolrConfig` (e.g. `$SOLR_HOME/blcSolrConfig`)
- From `$SOLR_HOME/example/solr`, copy `solr.xml` to `blcSolrConfig`
- From `$SOLR_HOME/example/solr/collection1`, copy `solrconfig.xml` and `elevate.xml` to `blcSolrConfig`
- From the Broadleaf Demo Site distribution, copy ```site/src/main/resources/schema.xml` to `blcSolrConfig`
- Now you are ready to let Zookeeper know about Solr's configuration files using a Solr [utility for Zookeeper](https://cwiki.apache.org/confluence/display/solr/Command+Line+Utilities)
- Make sure Zookeeper is running and execute the following command from the command line: 

```
./$SOLR_HOME/example/scripts/cloud-scripts/zkcli.sh -zkhost localhost:2181 -cmd upconfig -confname blc -confdir /path/to/blcSolrConfig
```

The above command (`zkcli.sh`) may require that you have aleady run Solr as a stand-alone server (using the `$SOLR_HOME/example/start.jar`), or that you have unzipped the `$SOLR_HOME/example/webapps/solr.war` file to `$SOLR_HOME/examplesolr-webapp/webapp`).  This is a bit awkward, but the scripts(s) require the exploded war to be in that location.

- The above command uploads the contents of the `blcSolrConfig` directory to the Zookeeper Quorum using a configuration name of "blc" (more on this later)
- Now, Zookeeper has most of the files that Solr will need
- Create a directory called `blcSolrHome0` anywhere on the file system (e.g. `$SOLR_HOME/myServer/blcSolrHome0`)
- Copy the ```solr.xml``` file from `blcSolrConfig` to `blcSolrHome0`
- Create a directory called `blcSolrHome1` anywhere on the file system (e.g. `$SOLR_HOME/myServer/blcSolrHome1`)
- Copy the `solr.xml` file from `blcSolrConfig` to `blcSolrHome1`
- Start an instance of Solr on port 8983 connecting to the Zookeeper Quorum and using `blcSolrHome0` as the home directory: 

```
./$SOLR_HOME/bin/solr start -cloud -p 8983 -s /path/to/blcSolrHome0 -z localhost:2181,localhost:2182,localhost:2183
```


- Start an instance of Solr on port 8984 connecting to the Zookeeper Quorum and using `blcSolrHome1` as the home directory: 

```
./$SOLR_HOME/bin/solr start -cloud -p 8984 -s /path/to/blcSolrHome1 -z localhost:2181,localhost:2182,localhost:2183
```

- Now you have 2 nodes or instances of SolrCloud running, with a 3-node Quorum (cluster) of Zookeeper instances managing the Solr cluster

Note that each of the Zookeeper instances can be configured on a different physical machine, as can each of the SolrCloud instances.  The configuration name "blc" is used, when creating collections, to specify the configuration for the collection.  See the [collection API](https://cwiki.apache.org/confluence/display/solr/Collections+API) for more information on creating collections.  One thing to note: You do not need to create any collections or aliases at this point.  Broadleaf will evaluate your cluster state and create the necessary collections and aliases as needed.  That said, this is a good time to talk about collections and aliases in case you want to create them yourself.

SolrCloud allows you to create one or more collections.  A collection is an abstraction on top of a Solr core. It is like a core, except that it can be distributed across multiple Solr servers.  A collection can be aliased, meaning it can be assigned and referenced by another name.  If you do not create the proper collections and aliases, Broadleaf will create the following:

- A collection called `blcCollectionX`, where the "X" is a number selected by Broadleaf that is unique, starting at 0 (zero)
- An alias called `primary`, which is assigned to `blcCollectionX`
- A collection called `blcCollectionY`, where the "Y" is a number selected by Broadleaf that is unique, starting at 1 (one), assuming that there is already a "blcCollection0"
- An alias called `reindex`, which is assigned to `blcCollectionY`

In other words, Broadleaf requires an alias called "primary" and an alias called "reindex".  It requires each alias to be assigned to exactly 1 collection.  The name of the aliases is important.  The name of the collections are not.  If the aliases exist and are assigned to collections, then Broadleaf will not create any.  If they exist and are not assigned to collections, then Broadleaf will create the collections and assign the aliases to them.  If the aliases do not exist, then Broadleaf will create the collections and the aliases and make the necessary assignments.

If Broadleaf is configured to do full reindexing (using the `SolrIndexService`) it will reindex the collection aliased as "reindex" and then it will tell Solr to reassign the alias of the indices (essentially swapping aliases with their respective collections).  This is similar to the concept of core swapping on an embedded or stand-alone Solr configuration. Although Solr allows a single alias to point to multiple collections, it is important that aliases and collections serving Broadleaf have a one-to-one relationship.

You can create the collections and aliases in advance if you would like.  You can do this via a browser (see the [collection API](https://cwiki.apache.org/confluence/display/solr/Collections+API)).  If you do, it is recommended to create them as follows:

- "blcCollection0" : aliased as "primary"
- "blcCollection1" : aliased as "reindex"

This can be done using the following URLs in a browswer, for example to create the collections:

```
http://localhost:8983/solr/admin/collections?action=CREATE&name=blcCollection0&numShards=2&collection.configName=blc
```

```
http://localhost:8983/solr/admin/collections?action=CREATE&name=blcCollection1&numShards=2&collection.configName=blc
```

And to create the aliases:

```
http://localhost:8983/solr/admin/collections?action=CREATEALIAS&name=primary&collections=blcCollection0
```

```
http://localhost:8983/solr/admin/collections?action=CREATEALIAS&name=reindex&collections=blcCollection1
```

Notice, that you are only creating the collections and aliases on a single Solr node, but you will be able access the collections via their aliases on both nodes:

```
http://localhost:8983/solr/primary/select?q=*
```

```
http://localhost:8983/solr/reindex/select?q=*
```

```
http://localhost:8984/solr/primary/select?q=*
```

```
http://localhost:8984/solr/reindex/select?q=*
```

Remember that creating the collections and aliases is not necessary as Broadleaf will happily do this for you if they do not exist.


## Notes for deployment on Windows

The above instructions are generally portable to Windows.  However, there are a few things to note:

- You should use corresponding `*.cmd` or `*.bat` instead of `*.sh` files
- When uploading the the configuration to Zookeeper, on Windows, use the following:

```
%SOLR_HOME%\example\scripts\cloud-scripts\zkcli.bat -zkhost localhost:2181 -cmd upconfig -confname blc -confdir /path/to/blcSolrConfig
```

- When starting the first instance of Solr on a Windows machine, use the following:
 
```
%SOLR_HOME%\bin\solr.cmd start -p 8983 -s "C:\path\to\blcSolrHome0" -z "localhost:2181,localhost:2182,localhost:2183"
```

- And to start the second instance of Solr (on the same Windows machine) use the following:

```
%SOLR_HOME%\bin\solr.cmd start -p 8984 -s "C:\path\to\blcSolrHome1" -z "localhost:2181,localhost:2182,localhost:2183"
```
