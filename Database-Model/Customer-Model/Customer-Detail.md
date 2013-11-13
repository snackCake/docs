# Customer Detail

###Detailed ERD

[![Customer Detail](dataModel/CustomerDetailedERD.png)](_img/dataModel/CustomerDetailedERD.png)

###Tables

| Table                      | Related Entity | Description                                         |
|:---------------------------|:----------|:----------------------------------------------------|
|BLC\_ADDRESS                 | ^[javadoc:org/broadleafcommerce/profile/core/domain/Address]     | Contains address information, e.g. city, state, and postal code   |
|BLC\_CHALLENGE\_QUESTION      | [ChallengeQuestion.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/ChallengeQuestion.html)      | Question to present the user for password recovery purposes       |
|BLC\_COUNTRY                 | [Country.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Country.html)      | Contains country information, e.g. abbreviation and name          |
|BLC\_CUSTOMER                | [Customer.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Customer.html)      | Represents a customer in Broadleaf  |
|BLC\_CUSTOMER\_ADDRESS        | [CustomerAddress.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/CustomerAddress.html)      | Associates a customer to an address  |
|BLC\_CUSTOMER\_ATTRIBUTE      | [CustomerAttribute.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/CustomerAttribute.html)      | Holds name-value pairs of attributes for a customer  |
|BLC\_CUSTOMER\_PASSWORD\_TOKEN | [CustomerForgotPasswordSecurityToken.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/CustomerForgotPasswordSecurityToken.html)      | Holds token information for password recovery purposes  |
|BLC\_CUSTOMER\_PHONE          | [CustomerPhone.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/CustomerPhone.html)      | Associates a customer to a phone number  |
|BLC\_CUSTOMER\_ROLE           | [CustomerRole.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/CustomerRole.html)      | Associates a customer to a role  |
|BLC\_PHONE                   | [Phone.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Phone.html)      | Contains phone information, e.g. number and if it's active        |
|BLC\_CUSTOMER\_PAYMENT        | [CustomerPayment.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/CustomerPayment.html)  | Contains customer payment information.    |
|BLC\_ROLE                    | [Role.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/Role.html)      | Contains role information, e.g. role name  |
|BLC\_STATE                   | [State.java](http://javadoc.broadleafcommerce.org/current/profile/org/broadleafcommerce/profile/core/domain/State.html)      | Contains state information, e.g. abbreviation, name, and country  |

###Related Tables

| Table               | Related Entity    | Description                                                    |
|:--------------------|:--------------|:---------------------------------------------------------------|
|BLC\_LOCALE           | [Locale.java](http://javadoc.broadleafcommerce.org/current/common/org/broadleafcommerce/common/locale/domain/Locale.html)          | Contains locale information, such as code and if it's default  |
