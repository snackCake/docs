# Common

###Detailed ERD

[![Common Detail](dataModel/CommonDetailedERD.png)](_img/dataModel/CommonDetailedERD.png)

###Tables

| Table                      | Related Entity | Description                                         |
|:---------------------------|:----------|:----------------------------------------------------|
|BLC_STATE                   | ^[javadoc:org/broadleafcommerce/profile/core/domain/State]      | Contains state information, e.g. abbreviation, name, and country   |
|BLC_COUNTRY                 | ^[javadoc:org/broadleafcommerce/profile/core/domain/Country]      | Contains country information, e.g. abbreviation and name          |
|BLC_SITE                    | ^[javadoc:org/broadleafcommerce/common/site/domain/Site]      | Represents a site  |
|BLC_ID_GENERATION           | ^[javadoc:org/broadleafcommerce/profile/core/domain/IdGeneration]      | Holds unique identifier data for various types  |
|BLC_DATA_DRVN_ENUM          | ^[javadoc:org/broadleafcommerce/common/enumeration/domain/DataDrivenEnumeration]      | Holds the name for data-driven enumeration purposes  |
|BLC_DATA_DRVN_ENUM_VAL      | ^[javadoc:org/broadleafcommerce/common/enumeration/domain/DataDrivenEnumerationValue]      | Holds value items for data-driven enumeration purpose  |
|SEQUENCE_GENERATOR          | n/a      | Holds information for sequence generation  |
|BLC_LOCALE                  | ^[javadoc:org/broadleafcommerce/common/locale/domain/Locale]      | Contains locale information, such as code and if it's default  |
|BLC_CURRENCY                | ^[javadoc:org/broadleafcommerce/common/currency/domain/BroadleafCurrency]      | Contains currency information, such as code and if it's default  |

###Related Tables

| Table                | Related Entity    | Description                                                    |
|:---------------------|:--------------|:---------------------------------------------------------------|
|BLC_SANDBOX           | ^[javadoc:org/broadleafcommerce/common/sandbox/domain/SandBox]          | Contains sandbox instance data  |
