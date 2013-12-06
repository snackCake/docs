# Customer Detail

###Detailed ERD

[![Customer Detail](dataModel/CustomerDetailedERD.png)](_img/dataModel/CustomerDetailedERD.png)

###Tables

| Table                      | Related Entity | Description                                         |
|:---------------------------|:----------|:----------------------------------------------------|
|BLC\_ADDRESS                 | ^[javadoc:org/broadleafcommerce/profile/core/domain/Address]     | Contains address information, e.g. city, state, and postal code   |
|BLC\_CHALLENGE\_QUESTION      | ^[javadoc:org/broadleafcommerce/profile/core/domain/ChallengeQuestion]      | Question to present the user for password recovery purposes       |
|BLC\_COUNTRY                 | ^[javadoc:org/broadleafcommerce/profile/core/domain/Country]      | Contains country information, e.g. abbreviation and name          |
|BLC\_CUSTOMER                | ^[javadoc:org/broadleafcommerce/profile/core/domain/Customer]      | Represents a customer in Broadleaf  |
|BLC\_CUSTOMER\_ADDRESS        | ^[javadoc:org/broadleafcommerce/profile/core/domain/CustomerAddress]      | Associates a customer to an address  |
|BLC\_CUSTOMER\_ATTRIBUTE      | ^[javadoc:org/broadleafcommerce/profile/core/domain/CustomerAttribute]      | Holds name-value pairs of attributes for a customer  |
|BLC\_CUSTOMER\_PASSWORD\_TOKEN | ^[javadoc:org/broadleafcommerce/profile/core/domain/CustomerForgotPasswordSecurityToken]      | Holds token information for password recovery purposes  |
|BLC\_CUSTOMER\_PHONE          | ^[javadoc:org/broadleafcommerce/profile/core/domain/CustomerPhone]      | Associates a customer to a phone number  |
|BLC\_CUSTOMER\_ROLE           | ^[javadoc:org/broadleafcommerce/profile/core/domain/CustomerRole]      | Associates a customer to a role  |
|BLC\_PHONE                   | ^[javadoc:org/broadleafcommerce/profile/core/domain/Phone]      | Contains phone information, e.g. number and if it's active        |
|BLC\_CUSTOMER\_PAYMENT        | ^[javadoc:org/broadleafcommerce/profile/core/domain/CustomerPayment]  | Contains customer payment information.    |
|BLC\_ROLE                    | ^[javadoc:org/broadleafcommerce/profile/core/domain/Role]      | Contains role information, e.g. role name  |
|BLC\_STATE                   | ^[javadoc:org/broadleafcommerce/profile/core/domain/State]      | Contains state information, e.g. abbreviation, name, and country  |

###Related Tables

| Table               | Related Entity    | Description                                                    |
|:--------------------|:--------------|:---------------------------------------------------------------|
|BLC\_LOCALE           | ^[javadoc:org/broadleafcommerce/common/locale/domain/Locale]          | Contains locale information, such as code and if it's default  |
