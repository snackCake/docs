# Payment Security and PCI Compliance

## <a id="PCI"></a>PCI Compliance

It is up to you on how you want to store, handle, and process credit card information. You can either handle it yourself or leave it up to a third party payment gateway. Broadleaf is designed to handle both of these scenarios. If you are looking to avoid most PCI Compliance concerns, Broadleaf integrates with several popular Payment Gateways that offer integration methods wherein credit card info never hits your servers (e.g. PayPal, Authorize.net, Braintree, and CyberSource, etc...). As a general rule: If you don't read, store, or process the PAN (CC#) and/or exp date, then the level of PCI Compliance certification is minimal. If you even remotely touch the card number, then you need to undergo extensive PCI Compliance certification.

Again, storing Credit Card data or even sending that information to your servers is not something that we would normally recommend as the PCI auditing process can be difficult and expensive and 99% of the time you don't need it. If the requirement is really to just allow customers to save their credit card information, some external payment gateways have a "vault" feature where the customer credit card still never hits your server.

## <a id="Encryption"></a>Encryption

By default, Broadleaf Commerce employs a useless pass-through implementation for the encryption module. The DefaultEncryptionModule class simply passes through the values adding neither encryption or decryption. To add real encryption and decryption, users will need to add support for their own strategy. The first step is to create a custom implementation of EncryptionModule. The EncryptionModule interface is left purposefully simple to allow the greatest degree of flexibility in implementation. Once an implementation is prepared, then you'll need to notify Broadleaf Commerce to use your custom encryption module instead of the default one by assigning the key id `blEncryptionModule`.

```xml
<bean id="blEncryptionModule" class="com.mycompany.encryption.MyEncryptionModule"/>
```

Choosing a strategy for encryption and decryption of sensitive customer data is an important consideration, especially for PCI compliance. We suggest StrongKey, which serves to centrally manage symmetric encryption keys. Creating a custom encryption module that works in tandem with StrongKey represents a viable approach for key management and secure storage of customer data. Please refer to the StrongKey website for more information: http://www.strongkey.org/. 
