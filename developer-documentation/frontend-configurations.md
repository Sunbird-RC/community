---
description: Registry provides following configuration properties
---

# Frontend Configurations

Go to **config.json** file and update the value of of key as with your application details

```javascript
{
   "keycloak": {      // Add your keycloak configurations here
   "url": "domainUrl/auth",
   "clientId": "registry-frontend",
   "realm": "sunbird-rc"
   },
   "configFolder": "/assets/config/ui-config",
   "languageFolder": "/assets/i18n",
   "title": "Application name",  // Here you can change Application name
   "baseUrl": "domainUrl/registry/api/v1", // Replace with your API URL
   "domainName" : "https://demo-url.xiv.in", //Replace with Site Domain
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

* **Keycloak** : Add your keycloak configurations in this object
* **baseUrl**: Set the Api base url to this property
* **domainName** : Add your domain name to this property
* **schemaUrl** : Set the swagger api url to this property
* **footerText** : You can set footer label here
* **Languages** : If you wants to add multilingual support on your site, you need to add  language code in this language property and also need to create respective files in [src](https://github.com/Sunbird-RC/demo-donor-registry/tree/main/donor-registry/src)/[assets](https://github.com/Sunbird-RC/demo-donor-registry/tree/main/donor-registry/src/assets)/[i18n](https://github.com/Sunbird-RC/demo-donor-registry/tree/main/donor-registry/src/assets/i18n)/global/ and [src](https://github.com/Sunbird-RC/demo-donor-registry/tree/main/donor-registry/src)/[assets](https://github.com/Sunbird-RC/demo-donor-registry/tree/main/donor-registry/src/assets)/[i18n](https://github.com/Sunbird-RC/demo-donor-registry/tree/main/donor-registry/src/assets/i18n)/[local](https://github.com/Sunbird-RC/demo-donor-registry/tree/main/donor-registry/src/assets/i18n/local)/ folder with specific language translations
* **Default\_theme**  - In this object you can change your site color, logo etc
