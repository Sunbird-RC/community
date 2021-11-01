# Update An Entity

To update an entity, we need to make the following HTTP request:

## Request

```http
PUT /api/v1/{entity-type}/{id}
```

| Field          | In     | Type     | Description                  |
| -------------- | ------ | -------- | ---------------------------- |
| `content-type` | header | `string` | Set to `application/json`    |
| `entity-type`  | path   | `string` | The type of entity to modify |
| `id`           | id     | `string` | The ID of entity to modify   |
| `...`          | body   | `object` | The entity's data            |

## Response

This will update the entity in the registry and return the following object:

```json
{
	"id": "open-saber.registry.update",
	"ver": "1.0",
	"ets": 1634371946769,
	"params": {
		"resmsgid": "",
		"msgid": "d51e6e6a-027d-4a42-84bb-2ce00e31d993",
		"err": "",
		"status": "SUCCESSFUL",
		"errmsg": ""
	},
	"responseCode": "OK"
}
```

## Usage

### cURL

```sh
curl --location \
	--request 'PUT' \
	--header 'content-type: application/json' \
	--header 'authorization: bearer {access-token}' \
	--data-raw '{
		...updated-claims
	}' \
	'{registry-url}/api/v1/{entity-type}/{id}'
```

### HTTPie

```sh
echo '{
	...updated-claims
}' | http put \
	'{registry-url}/api/v1/{entity-type}/{id}' \
	'content-type: application/json' \
	'authorization: bearer {access-token}'
```
