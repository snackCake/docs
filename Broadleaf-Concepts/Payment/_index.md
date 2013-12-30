# Payment

Starting with Broadleaf 3.1, the payment domain and various concepts have been refactored. If you are on an older version of Broadleaf, please refer to the 3.1 migration docs to see how to upgrade your domain.

In 3.1, all Payment Modules have been refactored to be decoupled from the `Core` framework in order to better genericize and facilitate in the different types of communication mechanisms that the gateways employ. This means that the payment gateway implementations are only dependent on the `Common` framework Jar and will utilize a common Request and Response DTO to communicate with each other. This decoupling allows for upgrades to the framework to be independent of the modules and vice versa. But most importantly, it allows for easier isolated unit and integration testing for each module.

Below is a High Level Diagram of how the Payment Gateway Interfaces are structured and how they relate to the `Core` framework as well as the implementing Modules.



Please use the menu on the right to navigate through the different sections of this documentation.
