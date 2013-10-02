# Admin Security



###Detailed ERD

[![Admin Security Detail](dataModel/AdminSecurityDetailedERD.png)](_img/dataModel/AdminSecurityDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_ADMIN\_USER       | [AdminUser.java](http://javadoc.broadleafcommerce.org/current/open-admin-platform/org/broadleafcommerce/openadmin/server/security/domain/AdminUser.html)          | Represents an admin user.  |
|BLC\_ADMIN\_USER\_ROLE\_XREF | -      | Cross reference table that points to an admin user role.  |
|BLC\_ADMIN\_ROLE       | [AdminRole.java](http://javadoc.broadleafcommerce.org/current/open-admin-platform/org/broadleafcommerce/openadmin/server/security/domain/AdminRole.html)          | Represents an admin user role.  |
|BLC\_ADMIN\_USER\_PERMISSION\_XREF| - | Cross reference table that points to an admin user permission.  |
|BLC\_ADMIN\_PERMISSION | [AdminPermission.java](http://javadoc.broadleafcommerce.org/current/open-admin-platform/org/broadleafcommerce/openadmin/server/security/domain/AdminPermission.html)          | Represents an admin user permission.  |
|BLC\_ADMIN\_ROLE\_PERMISSION\_XREF| - | Cross reference table that points to an admin role permission.  |
|BLC\_ADMIN\_PERMISSION\_ENTITY   | [AdminPermissionQualifiedEntity.java](http://javadoc.broadleafcommerce.org/current/open-admin-platform/org/broadleafcommerce/openadmin/server/security/domain/AdminPermissionQualifiedEntity.html) | Represents an admin user permission entity.  |

###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_SANDBOX          | [SandBox.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/sandbox/domain/SandBox.html)          | Represents a sandbox.  |
