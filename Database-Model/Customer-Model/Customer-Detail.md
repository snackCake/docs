# Customer Detail

###Detailed ERD

[![Customer Detail](dataModel/CustomerDetailedERD.png)](_img/dataModel/CustomerDetailedERD.png)

###Tables

| Table                      | Related Entity | Description                                         |
|:---------------------------|:----------|:----------------------------------------------------|
|BLC_ADDRESS                 | ^[javadoc:org/broadleafcommerce/profile/core/domain/Address]     | Contains address information, e.g. city, state, and postal code   |
|BLC_CHALLENGE_QUESTION      | ^[javadoc:org/broadleafcommerce/profile/core/domain/ChallengeQuestion]      | Question to present the user for password recovery purposes       |
|BLC_COUNTRY                 | ^[javadoc:org/broadleafcommerce/profile/core/domain/Country]      | Contains country information, e.g. abbreviation and name          |
|BLC_CUSTOMER                | ^[javadoc:org/broadleafcommerce/profile/core/domain/Customer]      | Represents a customer in Broadleaf  |
|BLC_CUSTOMER_ADDRESS        | ^[javadoc:org/broadleafcommerce/profile/core/domain/CustomerAddress]      | Associates a customer to an address  |
|BLC_CUSTOMER_ATTRIBUTE      | ^[javadoc:org/broadleafcommerce/profile/core/domain/CustomerAttribute]      | Holds name-value pairs of attributes for a customer  |
|BLC_CUSTOMER_PASSWORD_TOKEN | ^[javadoc:org/broadleafcommerce/profile/core/domain/CustomerForgotPasswordSecurityToken]      | Holds token information for password recovery purposes  |
|BLC_CUSTOMER_PHONE          | ^[javadoc:org/broadleafcommerce/profile/core/domain/CustomerPhone]      | Associates a customer to a phone number  |
|BLC_CUSTOMER_ROLE           | ^[javadoc:org/broadleafcommerce/profile/core/domain/CustomerRole]      | Associates a customer to a role  |
|BLC_PHONE                   | ^[javadoc:org/broadleafcommerce/profile/core/domain/Phone]      | Contains phone information, e.g. number and if it's active        |
|BLC_CUSTOMER_PAYMENT        | ^[javadoc:org/broadleafcommerce/profile/core/domain/CustomerPayment]  | Contains customer payment information.    |
|BLC_ROLE                    | ^[javadoc:org/broadleafcommerce/profile/core/domain/Role]      | Contains role information, e.g. role name  |
|BLC_STATE                   | ^[javadoc:org/broadleafcommerce/profile/core/domain/State]      | Contains state information, e.g. abbreviation, name, and country  |

###Related Tables

| Table               | Related Entity    | Description                                                    |
|:--------------------|:--------------|:---------------------------------------------------------------|
|BLC_LOCALE           | ^[javadoc:org/broadleafcommerce/common/locale/domain/Locale]          | Contains locale information, such as code and if it's default  |
