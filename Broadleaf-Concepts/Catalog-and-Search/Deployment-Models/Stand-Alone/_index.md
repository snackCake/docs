# Stand Alone Solr Server

It is usually beneficial, and even necessary, to use Sorl in a stand-alone mode.  This is different than Embedded Solr because it allow Solr to run as a network service on its own or on a shared piece of hardware.  This allows you to scale Solr independently.  It allows all of your web application nodes to share a single Solr server.  This is different than Embedded Solr because when Solr is embedded it does not run as a networked resource.  It also does not let you query or manage it outside of the application.  Stand-alone Solr allows for these things.

## Installing a Stand Alone Solr Server

In order to use Solr in a stand-alone way, download the latest [Solr distribution](http://lucene.apache.org/solr/mirrors-solr-latest-redir.html) and unpack it to an appropriate location on the file system. Let's assume this is `/path/to/solr-5.0.0`  Then, do the following:

1. Create a folder, possibly outside of the Solr distribution, to hold configuration and index data.  We'll call this `solr.home`, and can be assigned by using the `-s` flag when starting Solr.  This is not the location of the installation, but of the data associated with a running instance.  For purposes of illustration, let's assume that this is `/path/to/solr/home`
2. Copy the solr.xml file from `/path/to/solr-5.0.0/server/solr/solr.xml` to `/path/to/solr/home/solr.xml`
3. Create a directory called `primary` and another directory called `reindex` under `/path/to/solr/home/`
4. Under the `primary` directory, create a file called `core.properties` and add the following line to the file: `name=primary`
5. Under the `primary` directory create a folder called `conf` and another called `data`
6. Copy the `solrconfig.xml` file from your project's `site/src/main/resources/solrconfig.xml` to `/path/to/solr/home/primary/conf/solrconfig.xml`
7. Copy the `schema.xml` file from your project's `site/src/main/resources/schema.xml` to `/path/to/solr/home/primary/conf/schema.xml`
8. Under the `reindex` directory, create a file called `core.properties` and add the following line to the file: `name=reindex`
9. Under the `reindex` directory create a folder called `conf` and another called `data`
10. Copy the `solrconfig.xml` file from your project's `site/src/main/resources/solrconfig.xml` to `/path/to/solr/home/reindex/conf/solrconfig.xml`
11. Copy the `schema.xml` file from your project's `site/src/main/resources/schema.xml` to `/path/to/solr/home/reindex/conf/schema.xml`

## Starting a Stand Alone Solr Server
Now that Solr has been generally configured, and has been specifically pre-configured with 2 cores (`primary` and `reindex`), each with their own copy of Broadleaf's `solrconfig.xml` and `schema.xml`, you can start the Solr server.  To do so, you can `cd` to `/path/to/solr-5.0.0/bin` and execute the following command: `./solr -p 8983 -s /path/to/solr/home`.  This will start Solr on port 8983 and will tell Solr to use your newly configured Solr home directory for its configuration and data.  Doing this also sets `-Dsolr.home=/path/to/solr/home` as a system property. This allows you to start multiple instances of Solr on a single machine, each pointing to a different `solr.home` directory.

You can test your configuration by going `http://localhost:8983/solr/primary/select?q=*` or `http://localhost:8983/solr/reindex/select?q=*` in your browser, or using curl.  This should return an empty result, assuming you don't have any data in your index.

To stop Solr, you can `cd` to `/path/to/solr-5.0.0/bin` and call `./solr stop -p 8983`. This stops the Solr server instance listening on port 8983.

## Additional Notes
- See [Installing Solr](https://cwiki.apache.org/confluence/display/solr/Installing+Solr)
- See [Running Solr](https://cwiki.apache.org/confluence/display/solr/Running+Solr)
- See [Solr Start Script Reference](https://cwiki.apache.org/confluence/display/solr/Solr+Start+Script+Reference)
- See [Taking Solr To Production](https://cwiki.apache.org/confluence/display/solr/Taking+Solr+to+Production)

The solrconfig.xml and schema.xml files that are copied from Broadleaf are starting points or recommended configurations.  They can be modified as necessary for your specific needs.
