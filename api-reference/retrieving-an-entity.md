# Retrieving An Entity

## Request

To retrieve an entity, we need to make the following HTTP request:

```http
GET /api/v1/{entity-type}/{id} HTTP/1.1
```

| Field           | In     | Type     | Description                      |
| --------------- | ------ | -------- | -------------------------------- |
| `content-type`  | header | `string` | Set to `application/json` to retrieve in json. Other allowed values include `application/vc+ld+json`, `application/ld+json`, `application/pdf`       |
| `template-key` | header | `string` | Set appropriate value. Only required in case of `content-type` being `application/pdf`
| `authorization` | header | `string` | Set to `Bearer {access-token}`   |
| `entity-type`   | path   | `string` | The type of entity to retrieve   |
| `id`            | path   | `string` | The ID of the entity to retrieve |

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
	--header 'authorization: Bearer {access-token}' \
	'{registry-url}/api/v1/{entity-type}/{id}'
```

### HTTPie

```sh
http get \
	'{registry-url}/api/v1/{entity-type}/{id}' \
	'authorization: Bearer {access-token}'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found
> under the `rg` section in the `docker-compose.yaml` file and is usually
> `8081`.
