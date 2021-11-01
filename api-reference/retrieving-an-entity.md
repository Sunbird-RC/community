# Retrieving An Entity

## Request

To retrieve an entity, we need to make the following HTTP request:

```http
GET /api/v1/{entity-type}/{id} HTTP/1.1
```

| Field           | In     | Type     | Description                                                                                                    |
| --------------- | ------ | -------- | -------------------------------------------------------------------------------------------------------------- |
| `content-type`  | header | `string` | Set to `application/json`                                                                                      |
| `authorization` | header | `string` | Set to `bearer {access-token}` (substitute the access token for the one retrieved in the authentication step ) |
| `entity-type`   | path   | `string` | The type of entity to retrieve                                                                                 |
| `id`            | path   | `string` | The ID of the entity to retrieve                                                                               |

## Response

This will return the entity's JSON representation as follows:

```json
{
	"phoneNumber": "1234567890",
	"school": "UP Public School",
	"subject": "Math",
	"name": "Pranav Agate",
	"osid": "{id}",
	"osOwner": ["{owner-id}"],
	"_osState/school": "DRAFT"
}
```

Important variables in the response body:

| Field              | In   | Type     | Description                                                                                                                                                                                                                            |
| ------------------ | ---- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `osOwner`          | body | `string` | User ID of the entity in Keycloak.                                                                                                                                                                                                     |
| `_osState/{field}` | body | `string` | State of an attestable field. Can be `DRAFT` (when it has not been sent for attestation), `ATTESTATION_REQUESTED` (when sent for attestation), `PUBLISHED` (when successfully attested) and `REJECTED` (when rejected by the attestor) |

## Usage

### cURL

```sh
curl --location \
	--request GET \
	--header 'content-type: application/json' \
	--header 'authorization: bearer {access-token}' \
	'{registry-url}/api/v1/{entity-type}/{id}'
```

### HTTPie

```sh
http get \
	'{registry-url}/api/v1/{entity-type}/{id}' \
	'authorization: bearer {access-token}'
```
