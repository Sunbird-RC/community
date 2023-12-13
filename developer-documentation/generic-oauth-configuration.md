---
description: >-
  This page provides information about integration an OpenID Connect (OIDC)
  compliant OAuth 2.0 identity provider
---

# Generic OAuth Configuration

#### Environment variables

Registry environment variables specific to configure an OAuth 2.0 Identity Provider

| Properties                             | Description                                                                                                                                                                                                                                                                                                                                                 |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| identity\_provider                     | <p>name of the class which implements <code>dev.sunbirdrc.registry.identity_providers.providers.IdentityProvider</code> for OAuth. below are two implementation in sunbird rc for keycloak and fusion auth respectively <code>dev.sunbirdrc.auth.keycloak.KeycloakProviderImpl</code></p><p><code>dev.sunbirdrc.auth.genericiam.AuthProviderImpl</code></p> |
| sunbird\_sso\_url                      | provider connection url i.e. [`http://fusionauthwrapper:3990/fusionauth/api/v1/user`](http://fusionauthwrapper:3990/fusionauth/api/v1/user)                                                                                                                                                                                                                 |
| sunbird\_sso\_realm                    | realm name to be used for authentication and authorization                                                                                                                                                                                                                                                                                                  |
| sunbird\_sso\_admin\_client\_id        | client id to be used as admin                                                                                                                                                                                                                                                                                                                               |
| sunbird\_sso\_admin\_client\_secret    | secret key of admin client                                                                                                                                                                                                                                                                                                                                  |
| sunbird\_keycloak\_user\_set\_password | boolean value to default password for user/owner of entity in Identity Provider                                                                                                                                                                                                                                                                             |
| sunbird\_keycloak\_user\_password      | provide this value as true to set this as default user password                                                                                                                                                                                                                                                                                             |
| identity\_user\_actions                | actions which will be trigger by identity provider, example email actions: VERIFY\_EMAIL, UPDATE\_PROFILE, UPDATE\_PASSWORD,TERMS\_AND\_CONDITIONS etc. email details should be configured in keycloak realm settings                                                                                                                                       |
| oauth2\_resource\_uri                  | <p>OAuth2 resource URI<br>i.e. <a href="http://localhost:8080/auth/realms/sunbird-rc"><code>http://localhost:8080/auth/realms/sunbird-rc</code></a></p>                                                                                                                                                                                                     |
| oauth2\_resource\_email\_path          |  user email path in jwt token payload which should contain value of type string                                                                                                                                                                                                                                                                             |
| oauth2\_resource\_consent\_path        | user consent path in jwt token payload which should contain value of type map of string to integer                                                                                                                                                                                                                                                          |
| oauth2\_resource\_roles\_path          | user roles path in jwt token payload which should contain value of type list of string                                                                                                                                                                                                                                                                      |
| oauth2\_resource\_entity\_path         | entity name path in jwt token payload which should contain value of type list of string                                                                                                                                                                                                                                                                     |
| oauth2\_resource\_user\_id\_path       | user id path in jwt token payload which should contain value of type string                                                                                                                                                                                                                                                                                 |

#### Integrating the Keycloak indentity provider

| Property                               | Value                                                                                        |
| -------------------------------------- | -------------------------------------------------------------------------------------------- |
| identity\_provider                     | dev.sunbirdrc.auth.keycloak.KeycloakProviderImpl                                             |
| sunbird\_sso\_url                      | [http://keycloak:8080/auth](http://keycloak:8080/auth)                                       |
| sunbird\_sso\_realm                    | sunbird-rc                                                                                   |
| sunbird\_sso\_admin\_client\_id        | admin-api                                                                                    |
| sunbird\_sso\_admin\_client\_secret    | \*\*\*\*\*\*                                                                                 |
| sunbird\_keycloak\_user\_set\_password | abcd@123                                                                                     |
| sunbird\_keycloak\_user\_password      | true                                                                                         |
| identity\_user\_actions                |                                                                                              |
| oauth2\_resource\_uri                  | [http://localhost:8080/auth/realms/sunbird-rc](http://localhost:8080/auth/realms/sunbird-rc) |
| oauth2\_resource\_email\_path          | email                                                                                        |
| oauth2\_resource\_consent\_path        | consent                                                                                      |
| oauth2\_resource\_roles\_path          | realm\_access.roles                                                                          |
| oauth2\_resource\_entity\_path         | entity                                                                                       |
| oauth2\_resource\_user\_id\_path       | sub                                                                                          |

#### Integrating the FusionAuth indentity provider

| Property                               | Value                                                                                                        |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| identity\_provider                     | `dev.sunbirdrc.auth.genericiam.AuthProviderImpl`                                                             |
| sunbird\_sso\_url                      | [http://fusionauthwrapper:3990/fusionauth/api/v1/user](http://fusionauthwrapper:3990/fusionauth/api/v1/user) |
| sunbird\_sso\_realm                    | sunbird-rc                                                                                                   |
| sunbird\_sso\_admin\_client\_id        | admin-api                                                                                                    |
| sunbird\_sso\_admin\_client\_secret    | \*\*\*\*\*\*                                                                                                 |
| sunbird\_keycloak\_user\_set\_password | abcd@123                                                                                                     |
| sunbird\_keycloak\_user\_password      | true                                                                                                         |
| identity\_user\_actions                |                                                                                                              |
| oauth2\_resource\_uri                  | [http://fusionauth:9011/](http://fusionauth:9011/)                                                           |
| oauth2\_resource\_email\_path          | email                                                                                                        |
| oauth2\_resource\_consent\_path        | consent                                                                                                      |
| oauth2\_resource\_roles\_path          | roles                                                                                                        |
| oauth2\_resource\_entity\_path         | entity                                                                                                       |
| oauth2\_resource\_user\_id\_path       | sub                                                                                                          |

Additionally you can refer to this sample of Fusion Auth service on how to setup fusionauth [https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/services/sample-fusionauth-service/docker-compose.yml](https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/services/sample-fusionauth-service/docker-compose.yml)
