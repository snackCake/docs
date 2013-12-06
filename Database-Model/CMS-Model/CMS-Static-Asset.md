# CMS Static Asset



###Detailed ERD

[![CMS Static Asset Detail](dataModel/CMSStaticAssetDetailedERD.png)](_img/dataModel/CMSStaticAssetDetailedERD.png)

###Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC\_STATIC\_ASSET     | ^[javadoc:org/broadleafcommerce/cms/file/domain/StaticAsset]          | Represents a Static Asset in Broadleaf.  |
|BLC\_IMG\_STATIC\_ASSET | ^[javadoc:org/broadleafcommerce/cms/file/domain/ImageStaticAsset]          | Represents a Image Static Asset in Broadleaf.  |
|BLC\_STATIC\_ASSET\_DESC| ^[javadoc:org/broadleafcommerce/cms/file/domain/StaticAssetDescription]          | Defines a Description for a Static Asset.  |
|BLC\_ASSET\_DESC\_MAP   | -          | Maps a Static Asset to a Description.  |
|BLC\_STATIC\_ASSET\_STRG| ^[javadoc:org/broadleafcommerce/cms/file/domain/StaticAssetStorage]          | Represents a Static Asset Storage Object in Broadleaf.  |

###Related Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC\_ADMIN\_USER       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminUser]          | Represents an admin user.  |
|BLC\_SANDBOX          | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBox]          | Represents a sandbox.  |
