# Admin Change Sets



###Detailed ERD

[![Admin Change Sets Detail](dataModel/AdminChangeSetsDetailedERD.png)](_img/dataModel/AdminChangeSetsDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_SANDBOX          | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBox]          | Represents a sandbox.  |
|BLC_ADMIN_USER_SANDBOX   | -      | Cross reference table that points to an admin user.  |
|BLC_SITE_SANDBOX     | -          | Cross reference table that points to a site.  |
|BLC_SANDBOX_ITEM     | ^[javadoc:org/broadleafcommerce/openadmin/server/domain/SandBoxItem]          | Represents a sandbox item.  |
|SANDBOX_ITEM_ACTION  | -          | Cross reference table that points to an action.  |
|BLC_SANDBOX_ACTION   | ^[javadoc:org/broadleafcommerce/openadmin/server/domain/SandBoxAction]          | Represents a sandbox action.  |

###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC_ADMIN_USER       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminUser]          | Represents an admin user.  |
|BLC_SITE             | ^[javadoc:org/broadleafcommerce/common/site/domain/Site]         | Represents a site.  |
