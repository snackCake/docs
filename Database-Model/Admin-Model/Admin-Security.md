# Admin Security



###Detailed ERD

[![Admin Security Detail](dataModel/AdminSecurityDetailedERD.png)](_img/dataModel/AdminSecurityDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_ADMIN\_USER       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminUser]          | Represents an admin user.  |
|BLC\_ADMIN\_USER\_ROLE\_XREF | -      | Cross reference table that points to an admin user role.  |
|BLC\_ADMIN\_ROLE       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminRole]          | Represents an admin user role.  |
|BLC\_ADMIN\_USER\_PERMISSION\_XREF| - | Cross reference table that points to an admin user permission.  |
|BLC\_ADMIN\_PERMISSION | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminPermission]          | Represents an admin user permission.  |
|BLC\_ADMIN\_ROLE\_PERMISSION\_XREF| - | Cross reference table that points to an admin role permission.  |
|BLC\_ADMIN\_PERMISSION\_ENTITY   | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminPermissionQualifiedEntity] | Represents an admin user permission entity.  |

###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_SANDBOX          | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBox]          | Represents a sandbox.  |
