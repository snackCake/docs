# Common

###Detailed ERD

[![Common Detail](dataModel/CommonDetailedERD.png)](_img/dataModel/CommonDetailedERD.png)

###Tables

| Table                      | Related Entity | Description                                         |
|:---------------------------|:----------|:----------------------------------------------------|
|BLC\_STATE                   | ^[javadoc:org/broadleafcommerce/profile/core/domain/State]      | Contains state information, e.g. abbreviation, name, and country   |
|BLC\_COUNTRY                 | ^[javadoc:org/broadleafcommerce/profile/core/domain/Country]      | Contains country information, e.g. abbreviation and name          |
|BLC\_SITE                    | ^[javadoc:org/broadleafcommerce/common/site/domain/Site]      | Represents a site  |
|BLC\_ID\_GENERATION           | ^[javadoc:org/broadleafcommerce/profile/core/domain/IdGeneration]      | Holds unique identifier data for various types  |
|BLC\_DATA\_DRVN\_ENUM          | ^[javadoc:org/broadleafcommerce/common/enumeration/domain/DataDrivenEnumeration]      | Holds the name for data-driven enumeration purposes  |
|BLC\_DATA\_DRVN\_ENUM\_VAL      | ^[javadoc:org/broadleafcommerce/common/enumeration/domain/DataDrivenEnumerationValue]      | Holds value items for data-driven enumeration purpose  |
|SEQUENCE\_GENERATOR          | n/a      | Holds information for sequence generation  |
|BLC\_LOCALE                  | ^[javadoc:org/broadleafcommerce/common/locale/domain/Locale]      | Contains locale information, such as code and if it's default  |
|BLC\_CURRENCY                | ^[javadoc:org/broadleafcommerce/common/currency/domain/BroadleafCurrency]      | Contains currency information, such as code and if it's default  |
|BLC\_TRANSLATION             | ^[javadoc:org/broadleafcommerce/common/i18n/domain/Translation]      | Contains currency information, such as code and if it's default  |
|BLC\_MODULE\_CONFIGURATION    | ^[javadoc:org/broadleafcommerce/common/config/domain/ModuleConfiguration]      | Contains currency information, such as code and if it's default  |
|BLC\_SANDBOX    | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBox]      | Represents a Sandbox  |
|BLC\_SYSTEM\_PROPERTY    | ^[javadoc:org/broadleafcommerce/common/config/domain/SystemProperty]      | Represents a System Property (name/value pair) stored in the database  |
|BLC\_SANDBOX\_MGMT    | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBoxManagement]      | required mostly as a workaround for an issue in Hibernate, see javadocs for more details  |
|BLC\_SITE\_MAP\_CFG    | ^[javadoc:org/broadleafcommerce/common/sitemap/domain/SiteMapConfiguration]      | drives the building of the SiteMap. It contains general properties that drive the creation of the SiteMap such as directory paths, etc.  |
|BLC\_CUST\_SITE\_MAP\_GEN\_CFG    | ^[javadoc:org/broadleafcommerce/common/sitemap/domain/CustomUrlSiteMapGeneratorConfiguration]      | CustomSiteMapGenerator is controlled by this configuration  |
|BLC\_SITE\_MAP\_GEN\_CFG    | ^[javadoc:org/broadleafcommerce/common/sitemap/domain/SiteMapGeneratorConfiguration]      | URL tag generation controlled by this configuration. See javadocs for more details.  |
|BLC\_SITE\_MAP\_URL\_ENTRY   | ^[javadoc:org/broadleafcommerce/common/sitemap/domain/SiteMapUrlEntry]      | Represents a SiteMap URL Entry.  |

###Related Tables

| Table                | Related Entity    | Description                                                    |
|:---------------------|:--------------|:---------------------------------------------------------------|
|BLC\_SANDBOX           | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBox]          | Contains sandbox instance data  |
