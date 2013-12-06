# CMS Static Asset



###Detailed ERD

[![CMS Static Asset Detail](dataModel/CMSStaticAssetDetailedERD.png)](_img/dataModel/CMSStaticAssetDetailedERD.png)

###Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC_STATIC_ASSET     | ^[javadoc:org/broadleafcommerce/cms/file/domain/StaticAsset]          | Represents a Static Asset in Broadleaf.  |
|BLC_IMG_STATIC_ASSET | ^[javadoc:org/broadleafcommerce/cms/file/domain/ImageStaticAsset]          | Represents a Image Static Asset in Broadleaf.  |
|BLC_STATIC_ASSET_DESC| ^[javadoc:org/broadleafcommerce/cms/file/domain/StaticAssetDescription]          | Defines a Description for a Static Asset.  |
|BLC_ASSET_DESC_MAP   | -          | Maps a Static Asset to a Description.  |
|BLC_STATIC_ASSET_STRG| ^[javadoc:org/broadleafcommerce/cms/file/domain/StaticAssetStorage]          | Represents a Static Asset Storage Object in Broadleaf.  |

###Related Tables

| Table               | Java Docs      | Description                                         |
|:--------------------|:--------------|:----------------------------------------------------|
|BLC_ADMIN_USER       | ^[javadoc:org/broadleafcommerce/openadmin/server/security/domain/AdminUser]          | Represents an admin user.  |
|BLC_SANDBOX          | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBox]          | Represents a sandbox.  |
