# Generate admin token

## Pre-requisites

#### Steps to create an admin role

* Goto keycloak admin page, Ex: \<domain>/auth/
* The default admin credentials will be (admin / admin). Update the default admin password for security reasons.
* Goto roles section and add a `admin` role if not present

You can create a new user or update the existing user with the above role. A [client](generate-admin-token.md#steps-to-create-a-client-with-an-admin-role) can also be used if you have a backend service that will call the registry APIs.

#### Steps to create a user and attach an admin role

* Goto keycloak admin page, Ex: \<domain>/auth/
* Goto `users` section and click on `Add user` button.
* Fill in the required information and click on `Save` button
* Goto `Credential` section and provide the password for the user
* Once the user is created goto `Role Mappings` section
* Select `admin` role from the `Available Roles` and click on `Add selected`

**Use the below API to generate the token for the above user**

## Request

To authenticate as an entity, we need to make the following request:

## Generate token API &#x20;

<mark style="color:green;">`POST`</mark> `http:/keycloak-url/auth/realms/{realm}/protocol/openid-connect/token`

#### Headers

| Name                                           | Type   | Description                                |
| ---------------------------------------------- | ------ | ------------------------------------------ |
| content-type<mark style="color:red;">\*</mark> | String | Set to `application/x-www-form-urlencoded` |

#### Request Body

| Name                                          | Type   | Description                                                                                       |
| --------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------- |
| grant\_type<mark style="color:red;">\*</mark> | String | Set to `password`                                                                                 |
| client\_id<mark style="color:red;">\*</mark>  | String | Set to `registry-frontend`                                                                        |
| username<mark style="color:red;">\*</mark>    | String | The `_osConfig.ownershipAttributes.userId` of the entity according to the schema                  |
| password<mark style="color:red;">\*</mark>    | String | Set to `abcd@123` (default password, specified in registry's application.yml/docker compose file) |

{% tabs %}
{% tab title="200: OK " %}
```json
{
	"access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lk...2cSSaBKuB58I2OYDGw",
	"expires_in": 300,
	"not-before-policy": 0,
	"refresh_expires_in": 1800,
	"refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lk...9HulwVv12bBDUdU_nidZXo",
	"scope": "email profile",
	"session_state": "300f8a46-e430-4fd6-92aa-a2d337d7343e",
	"token_type": "Bearer"
}
```
{% endtab %}
{% endtabs %}

Important variables in the response body:

| Field          | In   | Type     | Description                                                        |
| -------------- | ---- | -------- | ------------------------------------------------------------------ |
| `access_token` | body | `string` | Access token used to retrieve/update entity                        |
| `expires_in`   | body | `number` | Number of seconds before the access token will be declared invalid |
| `token_type`   | body | `string` | Should be `Bearer`, else we have gotten the wrong token            |
| `scope`        | body | `string` | Using this token, what information we can access about the entity  |

## Usage

### cURL

```
curl --location \
	--request POST \
	--header 'content-type: application/x-www-form-urlencoded' \
	--data 'client_id=registry-frontend' \
	--data 'username={username}' \
	--data 'password=test' \
	--data 'grant_type=password' \
	'{keycloak-url}/auth/realms/{realm}/protocol/openid-connect/token'
```

### HTTPie

```
http --form post \
	'{keycloak-url}/auth/realms/{realm}/protocol/openid-connect/token' \
	'content-type: application/x-www-form-urlencoded' \
	'client_id=registry-frontend' \
	'username={username}' \
	'password=test' \
	'grant_type=password'
```

> `{keycloak-url}` is usually http://localhost:8080, and `{realm}` is usually `sunbird-rc`.
>
> The `{keycloak-url}` is usually `localhost:{port}`. The port can be found under the `keycloak` section in the `docker-compose.yml` file. The `{realm}` can be found at the top of the `realm-export.json` file used to configure keycloak.

#### Steps to create a client with an admin role

* Goto keycloak admin page
* Goto `clients` section and click on `Add` button
* Give a new client id and click on the save button
* Update the `access type` to `confidential`.
* Turn on `Service Accounts Enabled` button.
* Add a `Valid Redirect Url` Ex: [http://localhost:8081/auth](http://localhost:8081/auth)
* And click on the save button
* Goto `Service Account Roles` tab
* Select `admin` role from the `Available Roles` and click on `Add selected`
* Goto `Credentials` section and click on `Regenerate secret` and copy the value

**Use the below API to get the token for the above client created**

## Generate token api

<mark style="color:green;">`POST`</mark> `http:/keycloak-url/auth/realms/{realm}/protocol/openid-connect/token`

#### Headers

| Name                                           | Type   | Description                       |
| ---------------------------------------------- | ------ | --------------------------------- |
| Content-Type<mark style="color:red;">\*</mark> | String | application/x-www-form-urlencoded |

#### Request Body

| Name                                             | Type   | Description                                                 |
| ------------------------------------------------ | ------ | ----------------------------------------------------------- |
| grant\_type<mark style="color:red;">\*</mark>    |        | client\_credentials                                         |
| client\_id<mark style="color:red;">\*</mark>     | String | Use the client name that was used above                     |
| client\_secret<mark style="color:red;">\*</mark> | String | Use the client secret that was generated in the above steps |

{% tabs %}
{% tab title="200: OK " %}
```json
{
	"access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lk...2cSSaBKuB58I2OYDGw",
	"expires_in": 300,
	"not-before-policy": 0,
	"refresh_expires_in": 1800,
	"refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lk...9HulwVv12bBDUdU_nidZXo",
	"scope": "email profile",
	"session_state": "300f8a46-e430-4fd6-92aa-a2d337d7343e",
	"token_type": "Bearer"
}
```
{% endtab %}
{% endtabs %}
