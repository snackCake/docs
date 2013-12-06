# Admin Change Sets



###Detailed ERD

[![Admin Change Sets Detail](dataModel/AdminChangeSetsDetailedERD.png)](_img/dataModel/AdminChangeSetsDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_SANDBOX          | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBox]          | Represents a sandbox.  |
|BLC\_ADMIN\_USER\_SANDBOX   | -      | Cross reference table that points to an admin user.  |
|BLC\_SITE\_SANDBOX     | -          | Cross reference table that points to a site.  |
|BLC\_SANDBOX\_ITEM     | ^[javadoc:org/broadleafcommerce/openadmin/server/domain/SandBoxItem]          | Represents a sandbox item.  |
|SANDBOX\_ITEM\_ACTION  | -          | Cross reference table that points to an action.  |
|BLC\_SANDBOX\_ACTION   | ^[javadoc:org/broadleafcommerce/openadmin/server/domain/SandBoxAction]          | Represents a sandbox action.  |

###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_ADMIN\_USER       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminUser]          | Represents an admin user.  |
|BLC\_SITE             | ^[javadoc:org/broadleafcommerce/common/site/domain/Site]         | Represents a site.  |
