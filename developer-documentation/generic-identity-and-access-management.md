---
description: >-
  SunbirdRC was tightly coupled with keycloak for authentication & authorization
  of users. Currently, SunbirdRC is updated to support any identity layer
  (oauth2 compliant) for IAM
---

# Generic Identity And Access Management

> Generic IAM is part of release-1.0.0

SunbirdRC requires an IAM platform for mainly two purposes.

1. authN & authZ of users to enable trust between the user and the entity.&#x20;
2. to manage user accounts for the entities created

### Below steps will enable authenticating and authorizing tokens generated from any oauth2 complaint IAM service&#x20;

* Configure the below environment variables for the registry core service

```
- oauth2_resource_uri=https://domain/auth/
- oauth2_resource_email_path=email
- oauth2_resource_consent_path=consent
- oauth2_resource_roles_path=realm_access.roles
- oauth2_resource_entity_path=entity
```

`oauth2_resource_uri` should be configured with the domain url of the IAM service

**Example value**

**Keycloak: \`**[**https://keycloak-domain/auth/realms/sunbird-rc**](https://demo-education-registry.xiv.in/auth/realms/sunbird-rc)**\`**

**Auth0: \`**[**https://xxxx.us.auth0.com**](https://dev-i60gby3fxixns11k.us.auth0.com)**/\` (API Domain)**

**Fusionauth: \`http://domain/\` (The value of the issuer configured in the tenant page)**

`oauth2_resource_email_path` should be configured  with the path to be used for fetching email id from the token

`oauth2_resource_consent_path` (OPTIONAL) should be configured  with the _path_ to be used for fetching consent fields from the token

`oauth2_resource_roles_path` should be configured with the _path_ to be used for fetching roles  from the token&#x20;

`oauth2_resource_entity_path` (OPTIONAL) should be configured with the _path_ to be used for fetching entities from the token

### Steps to enable creating user accounts in any IAM platform

Currently, one needs to write a custom implementation to support creating users in the respective IAM platforms. SunbirdRC provided two ways to configure it:

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

### **1. Sub module in Sunbird RC**

Currently, SunbirdRC is shipped with two submodules (Keycloak & auth0) to integrate with IAM platform. [https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/java/middleware/registry-middleware](https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/java/middleware/registry-middleware).\
If you need support for any other platform then you would need to create a module in a similar fashion.\
You need to configure the below env with respective values

```
identity_provider: dev.sunbirdrc.auth.keycloak.KeycloakProviderImpl (Replace the value with your package name)
sunbird_sso_url: http://localhost:8080/auth/ (IAM url)
sunbird_sso_realm: (Optional)
sunbird_sso_admin_client_id: (Optional)
sunbird_sso_admin_client_secret: (Optional)
sunbird_keycloak_user_set_password: (Optional)
sunbird_keycloak_user_password: (Optional)
identity_user_actions: (Optional)
```

The module needs to be added to the core registry and you need to build a custom docker image and use it in your application.

**Steps to create a submodule in SunbirdRC:**

* Create a [submodule](https://spring.io/guides/gs/multi-module/) in java/middleware/registry-middleware
* Implement this provider [https://github.com/Sunbird-RC/sunbird-rc-core/blob/generic-auth/java/middleware/registry-middleware/identity-provider/src/main/java/dev/sunbirdrc/registry/identity\_providers/providers/IdentityProvider.java](https://github.com/Sunbird-RC/sunbird-rc-core/blob/generic-auth/java/middleware/registry-middleware/identity-provider/src/main/java/dev/sunbirdrc/registry/identity\_providers/providers/IdentityProvider.java), which returns the IdentityManager which handles user creation functionality. (Example [https://github.com/Sunbird-RC/sunbird-rc-core/blob/generic-auth/java/middleware/registry-middleware/keycloak/src/main/java/dev/sunbirdrc/auth/keycloak/KeycloakProviderImpl.java](https://github.com/Sunbird-RC/sunbird-rc-core/blob/generic-auth/java/middleware/registry-middleware/keycloak/src/main/java/dev/sunbirdrc/auth/keycloak/KeycloakProviderImpl.java))
* The IdentityManager should implement the [https://github.com/Sunbird-RC/sunbird-rc-core/blob/generic-auth/java/middleware/registry-middleware/identity-provider/src/main/java/dev/sunbirdrc/registry/identity\_providers/pojos/IdentityManager.java](https://github.com/Sunbird-RC/sunbird-rc-core/blob/generic-auth/java/middleware/registry-middleware/identity-provider/src/main/java/dev/sunbirdrc/registry/identity\_providers/pojos/IdentityManager.java), which handles user creation and returns the user id. (Example: [https://github.com/Sunbird-RC/sunbird-rc-core/blob/generic-auth/java/middleware/registry-middleware/keycloak/src/main/java/dev/sunbirdrc/auth/keycloak/KeycloakAdminUtil.java](https://github.com/Sunbird-RC/sunbird-rc-core/blob/generic-auth/java/middleware/registry-middleware/keycloak/src/main/java/dev/sunbirdrc/auth/keycloak/KeycloakAdminUtil.java))
* A Service Provider is configured and identified through a provider configuration file which we put in the resource directory META-INF/services. The file name is the fully-qualified name of the SPI and its content is the fully-qualified name of the SPI implementation. (Example: [https://github.com/Sunbird-RC/sunbird-rc-core/blob/generic-auth/java/middleware/registry-middleware/keycloak/src/main/resources/META-INF/services/dev.sunbirdrc.registry.identity\_providers.providers.IdentityProvider](https://github.com/Sunbird-RC/sunbird-rc-core/blob/generic-auth/java/middleware/registry-middleware/keycloak/src/main/resources/META-INF/services/dev.sunbirdrc.registry.identity\_providers.providers.IdentityProvider))
* Update `identity_provider` env with the provider package name\


### **2. Wrapper service**

Instead of creating a module in the core service, you can create an external/custom service which exposes an API to create users in your IAM platform.

The API should follow this API spec: [https://github.com/Sunbird-RC/sunbird-rc-core/blob/6a99ab9d564ef0518ff5fa8f6730a58e51808f6d/services/sample-fusionauth-service/api-spec.yml](https://github.com/Sunbird-RC/sunbird-rc-core/blob/6a99ab9d564ef0518ff5fa8f6730a58e51808f6d/services/sample-fusionauth-service/api-spec.yml)

A sample service to create a user in FusionAuth is provided. [https://github.com/Sunbird-RC/sunbird-rc-core/tree/6a99ab9d564ef0518ff5fa8f6730a58e51808f6d/services/sample-fusionauth-service](https://github.com/Sunbird-RC/sunbird-rc-core/tree/6a99ab9d564ef0518ff5fa8f6730a58e51808f6d/services/sample-fusionauth-service)

You need to configure the below env with respective values

```
identity_provider: dev.sunbirdrc.auth.genericiam.AuthProviderImpl
sunbird_sso_url: http://localhost:8080/auth/ (Replace the value with your service endpoint)
```



