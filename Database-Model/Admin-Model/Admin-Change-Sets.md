# Admin Change Sets



###Detailed ERD

[![Admin Change Sets Detail](dataModel/AdminChangeSetsDetailedERD.png)](_img/dataModel/AdminChangeSetsDetailedERD.png)

###Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_SANDBOX          | [SandBox.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/sandbox/domain/SandBox.html)          | Represents a sandbox.  |
|BLC\_ADMIN\_USER\_SANDBOX   | -      | Cross reference table that points to an admin user.  |
|BLC\_SITE\_SANDBOX     | -          | Cross reference table that points to a site.  |
|BLC\_SANDBOX\_ITEM     | [SandBoxItem.java](http://javadoc.broadleafcommerce.org/current/open-admin-platform/org/broadleafcommerce/openadmin/server/domain/SandBoxItem.html)          | Represents a sandbox item.  |
|SANDBOX\_ITEM\_ACTION  | -          | Cross reference table that points to an action.  |
|BLC\_SANDBOX\_ACTION   | [SandBoxAction.java](http://javadoc.broadleafcommerce.org/current/open-admin-platform/org/broadleafcommerce/openadmin/server/domain/SandBoxAction.html)          | Represents a sandbox action.  |

###Related Tables

| Table               | Related Entity    | Description                                         |
|:--------------------|:------------------|:----------------------------------------------------|
|BLC\_ADMIN\_USER       | [AdminUser.java](http://javadoc.broadleafcommerce.org/current/open-admin-platform/org/broadleafcommerce/openadmin/server/security/domain/AdminUser.html)          | Represents an admin user.  |
|BLC\_SITE             | [Site.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/site/domain/Site.html)         | Represents a site.  |
