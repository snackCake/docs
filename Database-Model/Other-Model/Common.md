# Common

###Detailed ERD

[![Common Detail](dataModel/CommonDetailedERD.png)](_img/dataModel/CommonDetailedERD.png)

###Tables

| Table                      | Related Entity | Description                                         |
|:---------------------------|:----------|:----------------------------------------------------|
|BLC\_STATE                   | [State.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/State.html)      | Contains state information, e.g. abbreviation, name, and country   |
|BLC\_COUNTRY                 | [Country.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Country.html)      | Contains country information, e.g. abbreviation and name          |
|BLC\_SITE                    | [Site.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/site/domain/Site.html)      | Represents a site  |
|BLC\_ID\_GENERATION           | [IdGeneration.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/IdGeneration.html)      | Holds unique identifier data for various types  |
|BLC\_DATA\_DRVN\_ENUM          | [DataDrivenEnumeration.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/enumeration/domain/DataDrivenEnumeration.html)      | Holds the name for data-driven enumeration purposes  |
|BLC\_DATA\_DRVN\_ENUM\_VAL      | [DataDrivenEnumerationValue.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/enumeration/domain/DataDrivenEnumerationValue.html)      | Holds value items for data-driven enumeration purpose  |
|SEQUENCE\_GENERATOR          | n/a      | Holds information for sequence generation  |
|BLC\_LOCALE                  | [Locale.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/locale/domain/Locale.html)      | Contains locale information, such as code and if it's default  |
|BLC\_CURRENCY                | [BroadleafCurrency.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/currency/domain/BroadleafCurrency.html)      | Contains currency information, such as code and if it's default  |
|BLC\_TRANSLATION             | [Translation.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/i18n/domain/Translation.html)      | Contains currency information, such as code and if it's default  |
|BLC\_MODULE\_CONFIGURATION    | [ModuleConfiguration.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/config/domain/ModuleConfiguration.html)      | Contains currency information, such as code and if it's default  |


###Related Tables

| Table                | Related Entity    | Description                                                    |
|:---------------------|:--------------|:---------------------------------------------------------------|
|BLC\_SANDBOX           | [SandBox.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/sandbox/domain/SandBox.html)          | Contains sandbox instance data  |
