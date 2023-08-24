---
description: Registry provides following configuration properties
---

# Frontend Configurations

Locate the `config.json` file in the root folder of your Sunbird RC project.&#x20;

Update the values of the keys according to your application details. Here's an example of how the updated `config.json` file might look:



```javascript
{
   "keycloak": {      // Add your keycloak configurations here
   "url": "domainUrl/auth",
   "clientId": "registry-frontend",
   "realm": "sunbird-rc"
   },
   "configFolder": "/assets/config/ui-config",
   "languageFolder": "/assets/i18n",
   "title": "Registry Name",  // Here you can change Application name
   "baseUrl": "domainUrl/registry/api/v1", // Replace with your API URL
   "domainName" : "https://demo-registry.xiv.in", //Replace with Site Domain
   "schemaUrl": "domainUrl/registry/api/docs/swagger.json", // Replace with your schema api(swagger) url
   "footerText": "Organ donation registry", // Change Footer text
   "languages": [
   "en"   
   ],
   "appType": "attestation",
   "default_theme": {  // Here you can change theme  color and site logo
         "logoPath": "../../assets/images/logo.svg",
          - - - 
          - - - 
      }

```



* **Keycloak**: Update the `url`, `clientId`, and `realm` properties with your Keycloak configuration details.
* **baseUrl**: Replace `"domainUrl/registry/api/v1"` with the actual URL of your API.
* **domainName**: Replace `"https://demo-registry.xiv.in"` with the domain name of your site.
* **schemaUrl**: Update `"domainUrl/registry/api/docs/swagger.json"` with the URL of your schema API (Swagger).
* **footerText**: Change the value of this property to the desired footer text for your application.
* **languages**: If you want to add multilingual support to your site, add the language codes you want to support in this array.
* **default\_theme**: Update the properties within this object to modify the site's color, logo etc.
