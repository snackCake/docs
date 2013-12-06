# Admin Security



###Detailed ERD

[![Admin Security Detail](dataModel/AdminSecurityDetailedERD.png)](_img/dataModel/AdminSecurityDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_ADMIN_USER       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminUser]          | Represents an admin user.  |
|BLC_ADMIN_USER_ROLE_XREF | -      | Cross reference table that points to an admin user role.  |
|BLC_ADMIN_ROLE       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminRole]          | Represents an admin user role.  |
|BLC_ADMIN_USER_PERMISSION_XREF| - | Cross reference table that points to an admin user permission.  |
|BLC_ADMIN_PERMISSION | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminPermission]          | Represents an admin user permission.  |
|BLC_ADMIN_ROLE_PERMISSION_XREF| - | Cross reference table that points to an admin role permission.  |
|BLC_ADMIN_PERMISSION_ENTITY   | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminPermissionQualifiedEntity] | Represents an admin user permission entity.  |

###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_SANDBOX          | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBox]          | Represents a sandbox.  |
