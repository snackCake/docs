# CMS Page



###Detailed ERD

[![CMS Page Detail](dataModel/CMSPageDetailedERD.png)](_img/dataModel/CMSPageDetailedERD.png)

###Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC\_PAGE             | ^[javadoc:org/broadleafcommerce/cms/page/domain/Page]          | Represents a static html Page in Broadleaf.  |
|BLC\_PAGE\_TEMPLATE    | ^[javadoc:org/broadleafcommerce/cms/page/domain/PageTemplate]          | Represents a Page template.  |
|BLC\_PAGE\_FLD         | ^[javadoc:org/broadleafcommerce/cms/page/domain/PageField]          | Represents a Page Field.  |
|BLC\_PAGE\_FLD\_MAP     | -          | Maps a Page to a Field.  |
|BLC\_PAGE\_RULE        | ^[javadoc:org/broadleafcommerce/cms/page/domain/PageRule]          | Represents a rule to be applied to a Page.  |
|BLC\_PAGE\_RULE\_MAP    | -          | Maps a Page to a Rule.  |
|BLC\_PAGE\_ITEM\_CRITERIA | ^[javadoc:org/broadleafcommerce/cms/page/domain/PageItemCriteria]        | Represents a Page item criteria.  |
|BLC\_QUAL\_CRIT\_PAGE\_XREF| -        | Cross reference table that points to a Page item criteria.  |

###Related Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC\_ADMIN\_USER       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminUser]          | Represents an admin user.  |
|BLC\_SANDBOX          | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBox]          | Represents a sandbox.  |
|BLC\_LOCALE           | ^[javadoc:org/broadleafcommerce/common/locale/domain/Locale]          | Contains locale information, such as code and if it's default  |
