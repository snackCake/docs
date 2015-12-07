###Build.xml Updates
####Admin
1. Replace the `tomcat-jrebel` target with the following:

    ```xml
    <target name="tomcat-jrebel">
        <delete dir="war/WEB-INF/lib"/>
        <artifact:mvn mavenHome="${maven.home}" fork="true">
            <jvmarg value="-XX:MaxPermSize=256M" />
            <jvmarg value="-Xmx1536M" />
            <jvmarg value="-Xdebug" />
            <jvmarg value="-Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n" />
            <jvmarg value="-Drebel.root=${project.root}" />
            <jvmarg value="-noverify" />
            <jvmarg value="-Drebel.check_class_hash=true" />
            <jvmarg value="-agentpath:${jrebel.path}" />
            <jvmarg value="-javaagent:${spring.instrument.path}" />
            <jvmarg value="-Druntime.environment=${runtime.environment}" />  
            <jvmarg value="-Ddatabase.user=${database.user}" />  
            <jvmarg value="-Ddatabase.password=${database.password}" />  
            <jvmarg value="-Ddatabase.driver=${database.driver}" />  
            <jvmarg value="-Ddatabase.url=${database.url}" />  
            <jvmarg value="-Dregistration.database.user=${database.user}" />  
            <jvmarg value="-Dregistration.database.password=${database.password}" />  
            <jvmarg value="-Dregistration.database.driver=${database.driver}" />  
            <jvmarg value="-Dregistration.database.url=${database.url}" />
            <arg value="compile"/>
            <arg value="war:exploded"/>
            <arg value="tomcat7:run"/>
        </artifact:mvn>
    </target>
    ```
1. Add the following target:

    ```xml
    <target name="tomcat-xrebel">
        <delete dir="war/WEB-INF/lib"/>
        <artifact:mvn mavenHome="${maven.home}" fork="true">
            <jvmarg value="-XX:MaxPermSize=256M" />
            <jvmarg value="-Xmx2048M" />
            <jvmarg value="-Xdebug" />
            <jvmarg value="-Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n" />
            <jvmarg value="-Drebel.root=${project.root}" />
            <jvmarg value="-noverify" />
            <jvmarg value="-Drebel.check_class_hash=true" />
            <jvmarg value="-agentpath:${jrebel.path}" />
            <jvmarg value="-javaagent:${xrebel.path}" />
            <jvmarg value="-javaagent:${spring.instrument.path}" />
            <jvmarg value="-Druntime.environment=${runtime.environment}" />
            <jvmarg value="-Ddatabase.user=${database.user}" />
            <jvmarg value="-Ddatabase.password=${database.password}" />
            <jvmarg value="-Ddatabase.driver=${database.driver}" />
            <jvmarg value="-Ddatabase.url=${database.url}" />
            <jvmarg value="-Dregistration.database.user=${database.user}" />
            <jvmarg value="-Dregistration.database.password=${database.password}" />
            <jvmarg value="-Dregistration.database.driver=${database.driver}" />
            <jvmarg value="-Dregistration.database.url=${database.url}" />
            <arg value="compile"/>
            <arg value="war:exploded"/>
            <arg value="tomcat7:run"/>
        </artifact:mvn>
    </target>
    ```

####Site
1. Replace the `tomcat-jrebel` target with the following:

    ```xml
    <target name="tomcat-jrebel">
        <delete dir="war/WEB-INF/lib"/>
        <artifact:mvn mavenHome="${maven.home}" fork="true">
            <jvmarg value="-XX:MaxPermSize=256M" />
            <jvmarg value="-Xmx1536M" />
            <jvmarg value="-Xdebug" />
            <jvmarg value="-Xrunjdwp:transport=dt_socket,address=8000,server=y,suspend=n" />
            <jvmarg value="-Drebel.root=${project.root}" />
            <jvmarg value="-noverify" />
            <jvmarg value="-Drebel.check_class_hash=true" />
            <jvmarg value="-agentpath:${jrebel.path}" />
            <jvmarg value="-javaagent:${spring.instrument.path}" />
            <jvmarg value="-Druntime.environment=${runtime.environment}" />  
            <jvmarg value="-Ddatabase.user=${database.user}" />  
            <jvmarg value="-Ddatabase.password=${database.password}" />  
            <jvmarg value="-Ddatabase.driver=${database.driver}" />  
            <jvmarg value="-Ddatabase.url=${database.url}" />
            <jvmarg value="-Dregistration.database.user=${database.user}" />  
            <jvmarg value="-Dregistration.database.password=${database.password}" />  
            <jvmarg value="-Dregistration.database.driver=${database.driver}" />  
            <jvmarg value="-Dregistration.database.url=${database.url}" />
            <arg value="compile"/>
            <arg value="war:exploded"/>
            <arg value="tomcat7:run"/>
        </artifact:mvn>
    </target>
    ```
1. Add the following target:

    ```xml
    <target name="tomcat-xrebel">
        <delete dir="war/WEB-INF/lib"/>
        <artifact:mvn mavenHome="${maven.home}" fork="true">
            <jvmarg value="-XX:MaxPermSize=256M" />
            <jvmarg value="-Xmx2048M" />
            <jvmarg value="-Xdebug" />
            <jvmarg value="-Xrunjdwp:transport=dt_socket,address=8000,server=y,suspend=n" />
            <jvmarg value="-Drebel.root=${project.root}" />
            <jvmarg value="-noverify" />
            <jvmarg value="-Drebel.check_class_hash=true" />
            <jvmarg value="-agentpath:${jrebel.path}" />
            <jvmarg value="-javaagent:${xrebel.path}" />
            <jvmarg value="-javaagent:${spring.instrument.path}" />
            <jvmarg value="-Druntime.environment=${runtime.environment}" />  
            <jvmarg value="-Ddatabase.user=${database.user}" />  
            <jvmarg value="-Ddatabase.password=${database.password}" />  
            <jvmarg value="-Ddatabase.driver=${database.driver}" />  
            <jvmarg value="-Ddatabase.url=${database.url}" />
            <jvmarg value="-Dregistration.database.user=${database.user}" />  
            <jvmarg value="-Dregistration.database.password=${database.password}" />  
            <jvmarg value="-Dregistration.database.driver=${database.driver}" />  
            <jvmarg value="-Dregistration.database.url=${database.url}" />
            <arg value="compile"/>
            <arg value="war:exploded"/>
            <arg value="tomcat7:run"/>
        </artifact:mvn>
    </target>
    ```
