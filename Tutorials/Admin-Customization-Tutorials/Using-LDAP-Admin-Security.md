#Using LDAP for Admin Security
Many orgizations use LDAP to store security credentials and wish to leverage existing LDAP infrastructure to manage Broadleaf Admin Users.  Spring Security makes this relatively easy.

##Steps
- In your project, navigate to and open admin/src/main/webapp/WEB-INF/applicationContext-admin-security.xml
- Add the following configuration:
```xml
<bean id="blUserDetailsMapper" class="org.broadleafcommerce.openadmin.server.security.ldap.BroadleafAdminLdapUserDetailsMapper">
    <property name="roleNameSubstitutions">
        <map>
            <!-- Note that you must map the LDAP role(s) to 1 or more Broadleaf roles -->
            <entry key="ROLE_MY_LDAP_ROLE" value="ROLE_ADMIN,ROLE_CMS"></entry>
        </map>
    </property>
</bean>
```
- The above configuration provides a custom mapping that takes the result of the authentication and allows for custom mappings to an AdminUserDetails object that works for both Spring and Broadleaf.  An important function of the BroadleafAdminLdapUserDetailsMapper is that it allows you to map LDAP roles to one or more Broadleaf Admin Roles.  Another important function is that the BroadleafAdminLdapUserDetailsMapper synchs the results of the mapping from LDAP into Broadleaf entities in the Broadleaf database.  Each time a user logs in via LDAP, the granted roles are refreshed for that user in Broadleaf.
- Next, you need to configure the LDAP Authentication Provider:
```xml
<!-- NOTE: Refer to Spring LDAP documentation to determine how to properly configure the contextSource and ldapAuthProvider for your specific LDAP configuration. -->
<bean id="contextSource"
      class="org.springframework.security.ldap.DefaultSpringSecurityContextSource">
    <constructor-arg value="ldap://ldaphost:port"/>
    <property name="userDn" value="${some.user.dn.string}"/>
    <property name="password" value="${some.ldap.password}"/>
</bean>

<bean id="ldapAuthProvider"
      class="org.springframework.security.ldap.authentication.LdapAuthenticationProvider">
    <constructor-arg>
        <bean class="org.springframework.security.ldap.authentication.BindAuthenticator">
            <constructor-arg ref="contextSource"/>
            <property name="userDnPatterns">
                <list>
                    <value>cn={0},ou=MyOU,ou=people,o=MyOrganization</value>
                    <value>cn={0},ou=MyUsers,ou=other,ou=people,o=MyOrganization</value>
                </list>
            </property>
        </bean>
    </constructor-arg>
    <constructor-arg>
        <bean class="org.springframework.security.ldap.userdetails.DefaultLdapAuthoritiesPopulator">
            <constructor-arg ref="contextSource"/>
            <constructor-arg value="ou=MyOU,ou=groups,o=MyOrganization"/>
            <property name="groupRoleAttribute" value="cn"/>
            <property name="groupSearchFilter" value="member={0}"/>
            <property name="searchSubtree" value="true"/>
        </bean>
    </constructor-arg>
    <property name="userDetailsContextMapper" ref="blUserDetailsMapper"></property>
    <property name="useAuthenticationRequestCredentials" value="true"></property>
</bean>
```
- Note that the blUserDetailsMapper is assigned to the ldapAuthProvider so that the correct mapping takes place after authentication is successful.
- Next, remove existing (default) blAdminAuthenticationManager configuration:
```xml
<sec:authentication-manager alias="blAdminAuthenticationManager">
    <sec:authentication-provider user-service-ref="blAdminUserDetailsService">
        <sec:password-encoder ref="blAdminPasswordEncoder">
            <sec:salt-source ref="blAdminSaltSource"/>
        </sec:password-encoder>
    </sec:authentication-provider>
</sec:authentication-manager>
```
- Replace the blAdminAuthenticationManager with a new configuration:
```xml
<sec:authentication-manager alias="blAdminAuthenticationManager">
    <sec:authentication-provider ref="ldapAuthProvider"/>
</sec:authentication-manager>
```
- Finally, build and deploy the admin and log in with a valid LDAP account

##Additional Notes
- The user name associated with the LDAP account will be used to create an Admin User in Broadleaf
- NAME, LOGIN, and EMAIL are required fields in the BLC\_ADMIN\_USER table. NAME and EMAIL are retrieved as follows:
```java
String email = (String) ctx.getObjectAttribute("mail");
String firstName = (String) ctx.getObjectAttribute("givenName");
String lastName = (String) ctx.getObjectAttribute("sn");
```
- If the first and last names are blank, then the username is used as the name

