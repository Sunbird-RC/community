---
description: >-
  This document will illustrate on how digilocker meripehchaan can be
  configured.
---

# Digilocker Meripehchaan SSO

### Assumptions

To get the Digilocker Meripehchaan SSO login button in the login page, you would need to use the keycloak theme instead of the custom theme provided by default.

### Pre-requisites

* Keycloak
* Digilocker partner account ([https://partners.digitallocker.gov.in/](https://partners.digitallocker.gov.in/))
* Generate client secrets in ([https://apisetu.gov.in/org/consumer/auth\_partners](https://apisetu.gov.in/org/consumer/auth\_partners))
* Set the redirect url to `<domain>/auth/realms/master/broker/oidc/endpoint`

### Steps to integrate Digilocker Meripehchaan SSO in keycloak

* Goto keycloak admin page `<domain>/auth/`
* Login with admin credentials
* Goto `Identity Providers`
* Click on `Add provider`
* Select `OpenID Connect v1.0`
* Enter the display name to be showed on the login page, Ex: `Login with Digilocker Meripehchaan`
* Set the Authorization URL to \`[https://digilocker.meripehchaan.gov.in/public/oauth2/1/authorize](https://digilocker.meripehchaan.gov.in/public/oauth2/1/authorize)\`
* Set the Token URL to \`[https://digilocker.meripehchaan.gov.in/public/oauth2/2/token](https://digilocker.meripehchaan.gov.in/public/oauth2/2/token)\`
* Turn on `Disable User Info` button
* Select `Client secret sent as post` from `` Client Authentication` `` options
* Set `Client Id` that was generated in Digilocker partner portal
* Set `Client Secret` that was generated in Digilocker partner portal
* Select `consent` from `Prompt` options
* Enable `Use PKCE` option
* Select `S256` from `PKCE Method` options

### Enable default keycloak theme

* Goto keycloak admin page `<domain>/auth/`
* Login with admin credentials
* Goto `clients -> registry-frontend`
* Select `keycloak` from `Login Theme` options
* Save the changes
