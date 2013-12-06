# CMS Structured Content



###Detailed ERD

[![CMS Structured Content Detail](dataModel/CMSStructuredContentDetailedERD.png)](_img/dataModel/CMSStructuredContentDetailedERD.png)

###Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC\_SC               | ^[javadoc:org/broadleafcommerce/cms/structure/domain/StructuredContent]          | Represents a Broadleaf Structured Content object.  |
|BLC\_SC\_TYPE          | ^[javadoc:org/broadleafcommerce/cms/structure/domain/StructuredContentType]          | Designates a Structured Content type.  |
|BLC\_SC\_FLD\_TMPLT     | ^[javadoc:org/broadleafcommerce/cms/structure/domain/StructuredContentFieldTemplate]          | Represents a Structured Content Field template.  |
|BLC\_SC\_FLD           | ^[javadoc:org/broadleafcommerce/cms/structure/domain/StructuredContentField]          | Represents a Structured Content Field.  |
|BLC\_SC\_FLD\_MAP       | -          | Maps a Structured Content Object to a Field.  |
|BLC\_SC\_RULE          | ^[javadoc:org/broadleafcommerce/cms/structure/domain/StructuredContentRule]          | Represents a rule to be applied to a Structured Content object.  |
|BLC\_SC\_RULE\_MAP      | -          | Maps a Structured Content Object to a Rule.  |
|BLC\_SC\_ITEM\_CRITERIA | ^[javadoc:org/broadleafcommerce/cms/structure/domain/StructuredContentItemCriteria]          | Represents a Structured Content item criteria.  |
|BLC\_QUAL\_CRIT\_SC\_XREF| -          | Cross reference table that points to an item criteria.  |

###Related Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC\_ADMIN\_USER       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminUser]          | Represents an admin user.  |
|BLC\_SANDBOX          | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBox]          | Represents a sandbox.  |
|BLC\_LOCALE           | ^[javadoc:org/broadleafcommerce/common/locale/domain/Locale]          | Contains locale information, such as code and if it's default  |
