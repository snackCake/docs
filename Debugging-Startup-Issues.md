# Debugging Startup Issues

Some errors that might have led you here:

```console
[artifact:mvn] Caused by: org.springframework.beans.factory.CannotLoadBeanClassException: Error loading class [com.mycompany.sample.payment.service.gateway.NullPaymentGatewayConfigurationImpl] for bean with name 'blNullPaymentGatewayConfiguration' defined in file [/Users/phillipverheyden/broadleaf/DemoSite/core/target/classes/com/mycompany/sample/payment/service/gateway/NullPaymentGatewayConfigurationImpl.class]: problem with class file or dependent class; nested exception is java.lang.UnsupportedClassVersionError: com/mycompany/sample/payment/service/gateway/NullPaymentGatewayConfigurationImpl : Unsupported major.minor version 52.0
[artifact:mvn]  at org.springframework.beans.factory.support.AbstractBeanFactory.resolveBeanClass(AbstractBeanFactory.java:1331)
```

```console
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.3:compile (default-compile) on project core: Fatal error compiling: invalid target release: 1.8 -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException
[ERROR]
[ERROR] After correcting the problems, you can resume the build with the command
[ERROR]   mvn <goals> -rf :core
```

Some potential solutions

1. Verify that `JAVA_HOME` points to a Java 8 JDK. If you are running from the command line, you can use `mvn -v` to see what version of Java Maven is pointing to
2. Verify that Eclipse references a Java 8 JDK


### Downgrading to Java 7

While we do not recommend using Java 7 as it is [end of life](https://java.com/en/download/faq/java_7.xml), various circumstances might require you to use it. In order to change the DemoSite to use Java 8:

1. Open up the root pom.xml in the demo project
2. Find the `maven-compiler-plugin` which should look like this:
    ```xml
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.3</version>
        <configuration>
            <source>1.8</source>
            <target>1.8</target>
        </configuration>
    </plugin>
    ```
3. Update the `source` and `target` to be `1.7` like below:
    ```xml
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.3</version>
        <configuration>
            <source>1.7</source>
            <target>1.7</target>
        </configuration>
    </plugin>
    ```

