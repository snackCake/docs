# CMS Structured Content



###Detailed ERD

[![CMS Structured Content Detail](dataModel/CMSStructuredContentDetailedERD.png)](_img/dataModel/CMSStructuredContentDetailedERD.png)

###Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC_SC               | ^[javadoc:org/broadleafcommerce/cms/structure/domain/StructuredContent]          | Represents a Broadleaf Structured Content object.  |
|BLC_SC_TYPE          | ^[javadoc:org/broadleafcommerce/cms/structure/domain/StructuredContentType]          | Designates a Structured Content type.  |
|BLC_SC_FLD_TMPLT     | ^[javadoc:org/broadleafcommerce/cms/structure/domain/StructuredContentFieldTemplate]          | Represents a Structured Content Field template.  |
|BLC_SC_FLD           | ^[javadoc:org/broadleafcommerce/cms/structure/domain/StructuredContentField]          | Represents a Structured Content Field.  |
|BLC_SC_FLD_MAP       | -          | Maps a Structured Content Object to a Field.  |
|BLC_SC_RULE          | ^[javadoc:org/broadleafcommerce/cms/structure/domain/StructuredContentRule]          | Represents a rule to be applied to a Structured Content object.  |
|BLC_SC_RULE_MAP      | -          | Maps a Structured Content Object to a Rule.  |
|BLC_SC_ITEM_CRITERIA | ^[javadoc:org/broadleafcommerce/cms/structure/domain/StructuredContentItemCriteria]          | Represents a Structured Content item criteria.  |
|BLC_QUAL_CRIT_SC_XREF| -          | Cross reference table that points to an item criteria.  |

###Related Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC_ADMIN_USER       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminUser]          | Represents an admin user.  |
|BLC_SANDBOX          | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBox]          | Represents a sandbox.  |
|BLC_LOCALE           | ^[javadoc:org/broadleafcommerce/common/locale/domain/Locale]          | Contains locale information, such as code and if it's default  |
