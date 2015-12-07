# REST API Changes

As of Broadleaf 4.0, we have standardized on using Spring MVC for all of our REST services. Previous versions of Broadleaf used Jersey 1, the reference implementation of the JAXRS 1.0 spec.

You have 2 options 

#####1. Migrate your REST APIs to use Spring MVC (recommended)

The upgrade for this is very straightforward and mostly entails find/replace operations.

1. Add a new class that extends from `BroadleafRestApiMvcConfiguration` annotated with `@Configuration`, `@EnableWebMvc` and an `@ComponentScan` that scans the package that your REST API endpoints are in. This class should look like this:

    ```java
    @Configuration
    @EnableWebMvc
    // TODO: replace "com.mycompany.api" with the package that contains my REST APIs
    @ComponentScan("com.mycompany.api")
    public class RestApiMvcConfiguration extends BroadleafRestApiMvcConfiguration {
    
    }
    ```
    
      > You can extend and override methods and beans as defined in `BroadleafRestApiMvcConfiguration`. For instance, you can override how Broadleaf configures Jackson's `ObjectMapper` class
  
      Add a new class that extends from `BroadleafSpringRestExceptionMapper` annotated with `@ControllerAdvice`. This class should look like this and should be within the same package from the `@ComponentScan` above:

    ```java
    package com.mycompany.api;
    
    @ControllerAdvice
    public class SpringRestExceptionMapper extends BroadleafSpringRestExceptionMapper {
    
    }
    ```
    

2. In `web.xml`, change the `RESTAPIServlet` to look like the following, changing the `contextConfigLocation` parameter to the fully-qualified class of the `@Configuration` class that you created above:

    ```xml
    <servlet>
      <servlet-name>RESTApiServlet</servlet-name>
         <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
         <init-param>
             <param-name>contextClass</param-name>
             <param-value>
                 org.springframework.web.context.support.AnnotationConfigWebApplicationContext
             </param-value>
         </init-param>
         <init-param>
             <param-name>contextConfigLocation</param-name>
             <param-value>
                 <!-- TODO: change to the correct package name for this configuration class -->
                 com.mycompany.api.RestApiMvcConfiguration
             </param-value>
         </init-param>
         <init-param>
            <!-- Ensures that 404s are mapped to @ControllerAdvice -->
            <param-name>throwExceptionIfNoHandlerFound</param-name>
            <param-value>true</param-value>
         </init-param>
         <load-on-startup>1</load-on-startup>
    </servlet>
    ```

3. Delete the following lines from `applicationContext-rest-api.xml`:

    ```xml
    <context:component-scan .../>
    
    <bean class="org.broadleafcommerce.core.web.api.BroadleafMessageBodyReaderWriter" scope="singleton"/>
    <bean class="org.broadleafcommerce.core.web.api.BroadleafRestExceptionMapper" scope="singleton"/>
    <bean class="org.broadleafcommerce.core.web.api.RestExceptionMapper" scope="singleton"/>
    ```
  
4. Update your endpoints to use Spring MVC annotations instead of JAXRS

      - Replace `@Path`, `@Produces` and `@Consumes` **at the controller level** with `@RestController` and `@RequestMapping`. 
      
      > Note that if you have a `consumes = ...` inside your `@RequestMapping` annotation at the controller level, all requests to that controller will require the header `Content-Type` to be set. The better approach is to add a `consumes` attribute to individual methods (on the `@RequestMapping` annotation) that require consumption of objects.
      
      - Replace `@GET`, `@DELETE`, `@PUT`, `@POST` and `@Path` annotations **at the method level** with `@RequestMapping` with the correct attributes. For instance, if you have a method annotated with `@POST @Path("{productId}"` then you should replace that with `@RequestMapping(value = "{productId}", method = RequestMethod.POST)`
      - Replace `@QueryParam` with `@RequestParam`. If any of the `@QueryParam`s are also annotated with `@DefaultValue`, add that as a `defaultValue` attribute to the `@RequestParam` annotation
      - Replace `@PathParam` with `@PathVariable`
      - Replace the `@Context URIInfo uriInfo` (like in `CartEndpoint.addProductToOrder`) with `@RequestParam MultiValueMap<String, String requestParams` and pass that to the super methods of `CartEndpoint.addProductToOrder` and `CartEndpoint.updateProductOptions`
      - Replace any `Response.Status.*.getStatusCode()` with `HttpStatus.*.value()` where * could be something like `INTERNAL_SERVER_ERROR`, `NOT_FOUND`, etc.
    

To see the full migration that was done on our DemoSite Heat Clinic for the REST APIs, [click here](https://github.com/BroadleafCommerce/DemoSite/commit/f861282185cdcad12a046f6f049bf1744e217c72)


##### 2. Continue to use JaxRS (not recommended)

If you would like to continue to use JAXRS for your REST services (like if if you have a lot of existing endpoints) you can do so with the following steps:

1. Add the Jersey dependencies to your `site/pom.xml`

    ```xml
    <dependency>
        <groupId>com.sun.jersey.contribs</groupId>
        <artifactId>jersey-spring</artifactId>
        <version>1.17.1</version>
        <exclusions>
            <exclusion>
                <groupId>org.springframework</groupId>
                <artifactId>spring</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.springframework</groupId>
                <artifactId>spring-core</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.springframework</groupId>
                <artifactId>spring-web</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.springframework</groupId>
                <artifactId>spring-beans</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context</artifactId>
            </exclusion>
            <exclusion>
                <artifactId>spring-aop</artifactId>
                <groupId>org.springframework</groupId>
            </exclusion>
        </exclusions>
    </dependency>
    ```

2. If you have an extension of the `CartEndpoint` class you will need to modify the `addProductToOrder` and `updateProductOptions` methods:

    a. Remove the `@Override` annotation since it no longer applies

    b. Invoke the super method but instead of passing in a `URIInfo` object, use the result of `JaxrsTypeConverterUtil.convertJaxRSUriInfoToParameterMap(uriInfo);`. For example, this is what the `addProductToOrder` method should look like when you are done (`updateProductOptions` is very similar):

        ```java
        @PUT
        @Path("items/{itemId}/options")
        public OrderWrapper updateProductOptions(@Context HttpServletRequest request,
                @Context UriInfo uriInfo,
                @PathParam("itemId") Long itemId,
                @QueryParam("priceOrder") @DefaultValue("true") boolean priceOrder) {
            MultiValueMap<String, String> requestParams = JaxrsTypeConverterUtil.convertJaxRSUriInfoToParameterMap(uriInfo);
            return super.updateProductOptions(request, requestParams, itemId, priceOrder);
        }
        ```

3.  In `site/web.xml`, replace

    ```xml
    <param-value>org.codehaus.jackson.jaxrs</param-value>
    ```
    with
    
    ```xml
    <param-value>com.fasterxml.jackson.jaxrs.json</param-value>
    ```
 


