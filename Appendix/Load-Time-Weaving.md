# Load Time Weaving in Broadleaf

For some of our modules (as well as some patch-release fixes) rely on Spring Instrument to transform the byte code of certain classes at runtime. Specifically, this allows us to modify existing Broadleaf Hibernate entities to add more fields and relationships without hindering custom extensions. For instance, the multi-tenant module adds a `catalog_id` column to the `BLC_SKU` table.

The demo site referenced from [[Getting Started]] has Spring instrument hooked up on the `jetty-demo` ant task. When you deploy Broadleaf to an external system you will need to have the Spring instrument jar included in your deployment. This is referenced as a -javaagent flag to the JVM. How this flag is passed depends on the startup arguments of your application server.

### Tomcat

In `setenv.sh`, include the following:

```bash
export CATALINA_OPTS="-Xmx1024m -javaagent:/full/path/to/spring-instrument.jar"
```

### Jetty

In `start.ini`, include

```bash
-javaagent:/full/path/to/spring-instrument.jar
```

### Glassfish

In `config/domain.xml`

```xml
<jvm-options>-javaagent:/full/path/to/spring-insrument.jar</jvm-options>
```

For all other application servers, consult the server documentation for how to pass custom JVM arguments to the instance.

[Spring instrument jar download](https://github.com/BroadleafCommerce/DemoSite/raw/master/lib/spring-instrument-3.2.2.RELEASE.jar)

> Note: the recommended version for Spring instrument might change as the version of Spring that Broadleaf targets changes
