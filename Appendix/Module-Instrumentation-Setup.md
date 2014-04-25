When using a module that requires JPA transformations, you must start your application server with the Spring instrumentation Java agent. This consist of passing in the following JVM arg:

    -javaagent:/path/to/spring-instrument.jar

When using the bundled Ant build scripts, this is taken care of for you - you simply have to configure the path to where the JAR lives on your system in `build.properties`.

When using a different application server, reference that server's documentation to deterimne the appropriate place to set that argument.

## Important Note

There have been known issues with instrumentation when the Spring Instrument JAR is located either in the Tomcat lib directory or inside of the WAR of the application.

It is *strongly* recommended to keep the spring-instrument JAR in a separate directory from Tomcat.
