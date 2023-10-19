# Generate token

We can authenticate with the registry as a particular entity to perform operations like retrieving, searching, updating and attesting.

## Request

To authenticate as an entity, we need to make the following request:

{% swagger method="post" path="http:/keycloak-url/auth/realms/{realm}/protocol/openid-connect/token" baseUrl=" " summary="" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to `application/x-www-form-urlencoded`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="grant_type" required="true" %}
Set to `password`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="client_id" required="true" %}
Set to `registry-frontend`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="username" required="true" %}
The `_osConfig.ownershipAttributes.userId` of the entity according to the schema
{% endswagger-parameter %}

{% swagger-parameter in="body" name="password" required="true" %}
Set to `abcd@123` (default password, specified in registry's application.yml/docker compose file)
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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
{% endswagger-response %}
{% endswagger %}

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
