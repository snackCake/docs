# Admin Modules

### Detailed ERD

[![Admin Modules Detail](dataModel/AdminModulesDetailedERD.png)](_img/dataModel/AdminModulesDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_ADMIN\_MODULE       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminModule]          | Represents an admin user.  |
|BLC\_ADMIN\_SECTION\_PERMISSION\_XREF | -      | Cross reference table that points to an admin permission.  |
|BLC\_ADMIN\_SECTION       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminSection]          | Represents an admin section.  |
|BLC\_ADMIN\_PERMISSION | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminPermission]          | Represents an admin user permission.  |

