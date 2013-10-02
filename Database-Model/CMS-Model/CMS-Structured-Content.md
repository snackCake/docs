# CMS Structured Content



###Detailed ERD

[![CMS Structured Content Detail](dataModel/CMSStructuredContentDetailedERD.png)](_img/dataModel/CMSStructuredContentDetailedERD.png)

###Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC\_SC               | [StructuredContent.java](http://javadoc.broadleafcommerce.org/current/contentmanagement-module/org/broadleafcommerce/cms/structure/domain/StructuredContent.html)          | Represents a Broadleaf Structured Content object.  |
|BLC\_SC\_TYPE          | [StructuredContentType.java](http://javadoc.broadleafcommerce.org/current/contentmanagement-module/org/broadleafcommerce/cms/structure/domain/StructuredContentType.html)          | Designates a Structured Content type.  |
|BLC\_SC\_FLD\_TMPLT     | [StructuredContentFieldTemplate.java](http://javadoc.broadleafcommerce.org/current/contentmanagement-module/org/broadleafcommerce/cms/structure/domain/StructuredContentFieldTemplate.html)          | Represents a Structured Content Field template.  |
|BLC\_SC\_FLD           | [StructuredContentField](http://javadoc.broadleafcommerce.org/current/contentmanagement-module/org/broadleafcommerce/cms/structure/domain/StructuredContentField.html)          | Represents a Structured Content Field.  |
|BLC\_SC\_FLD\_MAP       | -          | Maps a Structured Content Object to a Field.  |
|BLC\_SC\_RULE          | [StructuredContentRule.java](http://javadoc.broadleafcommerce.org/current/contentmanagement-module/org/broadleafcommerce/cms/structure/domain/StructuredContentRule.html)          | Represents a rule to be applied to a Structured Content object.  |
|BLC\_SC\_RULE\_MAP      | -          | Maps a Structured Content Object to a Rule.  |
|BLC\_SC\_ITEM\_CRITERIA | [StructuredContentItemCriteria.java](http://javadoc.broadleafcommerce.org/current/contentmanagement-module/org/broadleafcommerce/cms/structure/domain/StructuredContentItemCriteria.html)          | Represents a Structured Content item criteria.  |
|BLC\_QUAL\_CRIT\_SC\_XREF| -          | Cross reference table that points to an item criteria.  |

###Related Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC\_ADMIN\_USER       | [AdminUser.java](http://javadoc.broadleafcommerce.org/current/open-admin-platform/org/broadleafcommerce/openadmin/server/security/domain/AdminUser.html)          | Represents an admin user.  |
|BLC\_SANDBOX          | [SandBox.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/sandbox/domain/SandBox.html)          | Represents a sandbox.  |
|BLC\_LOCALE           | [Locale.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/locale/domain/Locale.html)          | Contains locale information, such as code and if it's default  |
