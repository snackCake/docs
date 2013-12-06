

###Detailed ERD

[![Admin Modules Detail](images/dataModel/AdminModulesDetailedERD.png)](images/dataModel/AdminModulesDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_ADMIN_MODULE       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminModule]          | Represents an admin user.  |
|BLC_ADMIN_SECTION_PERMISSION_XREF | -      | Cross reference table that points to an admin permission.  |
|BLC_ADMIN_SECTION       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminSection]          | Represents an admin section.  |
|BLC_ADMIN_PERMISSION | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminPermission]          | Represents an admin user permission.  |

