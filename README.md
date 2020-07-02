# Odoo OAuth authentication with Keycloak OpenID connect
In our scenario Keycloak acts as the OAuth service and Odoo as the application that delegates the user authentication. In this guide you learn how to configure Odoo and Keycloak to handle an implicit OAuth flow.

##### We assume that we have the following service up and running:
  1. Keycloak Auth Server: http://192.168.100.91:8080
  2. Odoo v11 Application: http://192.168.100.92:8069
  
## 1) Setup Keycloak client and users  
   a) Open the Keycloak management console, select your realm, navigate to Configure > Clients and create a new client:
```bash
    Client ID :  odoov11
    Client Protocol :  openid-connect
    Root URL  :  ${authBaseUrl}
    
    Click save.
```
  b) In the client edit view make the following configurations:
```bash
    Access type :   confidential
    Implicit Flow Enabled :   On
    Valid Redirect URIs:
      /realms/realm-name/account/*
      http://192.168.100.92:8069/auth_oauth/signin    
    Base URL:   /realms/realm-name/account/
    
    Leave the Admin URL and Web Origins empty.
    Click save.
````
   c) Open the Mappers tab. Click on Add Builtin. Select and add the email entry. Open the email mapper and set as   
```bash
    Token Claim Name:  user_id
    
    Click save.
```

   d) On your realm, navigate to Manage > Users and Add a new user:
```bash
    Username  :  genral73
    Email :  genral73@local.com
    First Name  : dafaallah
    Last Name : alomda
    
    Click save.
```

   e) Open the Credentials tab. Set the password as    
```bash
    Password:  ******
    Password Confirmation:  ******
    Temporary:  Off
    
    Click Set Password.
```

## 2) Add Keycloak provider in Odoo
  
   a) Download the module zip from https://github.com/Mint-System/Odoo-App-Auth-OAuth-Keycloak and install the module.
   b) Login to the Odoo dashboard and navigate to Settings > General Settings > Integrations.
      If necessary enable OAuth Authentication and then click on OAuth Providers.
      Create a new provider with the following settings:
```bash
    Provider Name:  Keycloak Provider
    Client ID:  odoov11
    Allowed:  [x]
    Keycloak:   [x]
    Body: Login vi Keycloak
    Authentication URL:  http://192.168.100.91:8080/auth/realms/realm-name/protocol/openid-connect/auth
    Scope:  email
    Validation URL:  http://192.168.100.91:8080/auth/realms/realm-name/protocol/openid-connect/userinfo    
    
    Click Save.
```
  c) Active the developer mode and navigate to Settings > General Settings > Users > Customer Account > Default Access Rights. Edit the template and select for User types: Internal User. Save the portal user template.













































