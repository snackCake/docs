#Overriding Admin Templates

In this tutorial we will be making every text field in the admin a WYSIWYG field, via the Redactor WYSIWYG editor. To achieve this, we will override the `string` form template.

> This tutorial is based off of the stock Heat Clinic but should be relevant to any Broadleaf project

In the Broadleaf Open Admin project, we have defined a Thymeleaf `ServletContextTemplateResolver`. The [bean definition](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/master/admin/broadleaf-open-admin-platform/src/main/resources/applicationContext-servlet-open-admin.xml) looks like this:

```xml
<bean id="blAdminWebTemplateResolver" class="org.thymeleaf.templateresolver.ServletContextTemplateResolver">
    <property name="prefix" value="/WEB-INF/templates/admin/" />
    <property name="suffix" value=".html" />
    <property name="templateMode" value="HTML5" />
    <property name="cacheable" value="${cache.page.templates}"/>
    <property name="cacheTTLMs" value="${cache.page.templates.ttl}" />
    <property name="characterEncoding" value="UTF-8" />
    <property name="order" value="200"/>         
</bean>
```

This servlet template resolver is configured to execute prior to any of the other template resolvers:

```xml
<bean id="blAdminWebTemplateResolvers" class="org.springframework.beans.factory.config.SetFactoryBean">
    <property name="sourceSet">
        <set>
            <ref bean="blAdminWebCustomTemplateResolver" />
            <ref bean="blAdminWebTemplateResolver" />
            <ref bean="blAdminWebClasspathTemplateResolver" />
            <ref bean="blWebCommonClasspathTemplateResolver" />
        </set>
    </property>
</bean>
```

which means that the admin will try to include templates from `/WEB-INF/templates/admin` before any other location.

> If you want to use a different folder you can create another template resolver bean and add it to the `blAdminWebTemplateResolvers` set

In order to determine which template path we should override, let's first look at the different templates that the admin provides.

![Admin Templates](admin-templates.png)

All of the admin field templates are in the 'fields' folder under templates. Since we want to modify all text fields, we will be overriding `string.html`.

> The field template is selected by the `fieldType` metatadata on the field itself

With this information, actually overriding the template itself can be completed with 2 easy steps:

1. Create your new `string` template in `src/main/webapp/WEB-INF/templates-admin/fields`

![Admin string template](admin-string-template.png)

2. Copy the contents of the [Broadleaf HTML field template](https://github.com/BroadleafCommerce/BroadleafCommerce/blob/master/admin/broadleaf-open-admin-platform/src/main/resources/open_admin_style/templates/fields/html.html) into this one

    ```html
    <div class="field-label inline" th:text="#{${field.friendlyName}}" th:classappend="${field.required ? 'required' : ''}" />
    <div th:substituteby="components/fieldTooltip" />
    <div th:substituteby="components/fieldTranslation" />
    <span class="error" th:errors="*{fields['__${field.name}__'].value}" />

    <div class="twelve indented-form-value"
        th:with="assetAssociationId=${entityForm.id==null? entityForm.parentId : entityForm.id}">

        <textarea class="redactor" name="content"
              th:if="${overrideAssetSectionKey}"
              th:attr="data-select-asset-url=@{${'/'+overrideAssetSectionKey+ '/' + assetAssociationId + '/chooseAsset'}}"
              th:field="*{fields['__${field.name}__'].value}"/>

        <textarea class="redactor" name="content"
              th:unless="${overrideAssetSectionKey}"
              th:attr="data-select-asset-url=@{${'/'+sectionKey+ '/' + assetAssociationId + '/chooseAsset'}}"
              th:field="*{fields['__${field.name}__'].value}"/>

    </div>
    ```

    > Note: this is up to date as of Broadleaf 3.1.1-GA. This may differ slightly depending on the version of Broadleaf you are using

You can utilize this same paradigm to override any other templates in the admin, as long as the path matches the same template path that we have defined in Broadleaf.
