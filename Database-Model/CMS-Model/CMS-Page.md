# CMS Page



###Detailed ERD

[![CMS Page Detail](dataModel/CMSPageDetailedERD.png)](_img/dataModel/CMSPageDetailedERD.png)

###Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC_PAGE             | ^[javadoc:org/broadleafcommerce/cms/page/domain/Page]          | Represents a static html Page in Broadleaf.  |
|BLC_PAGE_TEMPLATE    | ^[javadoc:org/broadleafcommerce/cms/page/domain/PageTemplate]          | Represents a Page template.  |
|BLC_PAGE_FLD         | ^[javadoc:org/broadleafcommerce/cms/page/domain/PageField]          | Represents a Page Field.  |
|BLC_PAGE_FLD_MAP     | -          | Maps a Page to a Field.  |
|BLC_PAGE_RULE        | ^[javadoc:org/broadleafcommerce/cms/page/domain/PageRule]          | Represents a rule to be applied to a Page.  |
|BLC_PAGE_RULE_MAP    | -          | Maps a Page to a Rule.  |
|BLC_PAGE_ITEM_CRITERIA | ^[javadoc:org/broadleafcommerce/cms/page/domain/PageItemCriteria]        | Represents a Page item criteria.  |
|BLC_QUAL_CRIT_PAGE_XREF| -        | Cross reference table that points to a Page item criteria.  |

###Related Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC_ADMIN_USER       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminUser]          | Represents an admin user.  |
|BLC_SANDBOX          | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBox]          | Represents a sandbox.  |
|BLC_LOCALE           | ^[javadoc:org/broadleafcommerce/common/locale/domain/Locale]          | Contains locale information, such as code and if it's default  |
