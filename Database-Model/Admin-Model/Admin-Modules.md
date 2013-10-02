# Admin Modules

### Detailed ERD

[![Admin Modules Detail](dataModel/AdminModulesDetailedERD.png)](_img/dataModel/AdminModulesDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_ADMIN\_MODULE       | [AdminModule.java](http://javadoc.broadleafcommerce.org/current/open-admin-platform/org/broadleafcommerce/openadmin/server/security/domain/AdminModule.html)          | Represents an admin user.  |
|BLC\_ADMIN\_SECTION\_PERMISSION\_XREF | -      | Cross reference table that points to an admin permission.  |
|BLC\_ADMIN\_SECTION       | [AdminSection.java](http://javadoc.broadleafcommerce.org/current/open-admin-platform/org/broadleafcommerce/openadmin/server/security/domain/AdminSection.html)          | Represents an admin section.  |
|BLC\_ADMIN\_PERMISSION | [AdminPermission.java](http://javadoc.broadleafcommerce.org/current/open-admin-platform/org/broadleafcommerce/openadmin/server/security/domain/AdminPermission.html)          | Represents an admin user permission.  |

