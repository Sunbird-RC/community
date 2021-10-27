# Creating An Entity

To create an entity, we need to make the following HTTP request:

## Request

```http
POST /api/v1/{entity-type}/invite
```

| Field          | In     | Type     | Description                  |
| -------------- | ------ | -------- | ---------------------------- |
| `content-type` | header | `string` | Set to `application/json`    |
| `entity-type`  | path   | `string` | The type of entity to create |
| `...`          | body   | `object` | The entity's data            |

## Response

This will store the entity in the registry and return the following object:

```json
{
	"id": "open-saber.registry.invite",
	"ver": "1.0",
	"ets": 1634198998956,
	"params": {
		"resmsgid": "",
		"msgid": "3ee6a76f-d6c8-4262-a7ee-ddbe66fcb127",
		"err": "",
		"status": "SUCCESSFUL",
		"errmsg": ""
	},
	"responseCode": "OK",
	"result": { "Teacher": { "osid": "1-9d6099fc-2c01-4714-bceb-55ff28c482f9" } }
}
```

Important variables in the response body:

| Field                       | In   | Type     | Description                                                                                    |
| --------------------------- | ---- | -------- | ---------------------------------------------------------------------------------------------- |
| `result.{entity-type}.osid` | body | `string` | The ID of the create entity in the registry, used for retrieval and modification of the entity |

## Usage

So to create a `Teacher` entity named Pranav Agate who teaches Math at UP Public
School, we would make the following API call:

### cURL

```sh
curl --location \
	--request 'POST' \
	--header 'content-type: application/json' \
	--data-raw '{
		...claims
	}' \
	'{registry-url}/api/v1/{entity-type}/invite'
```

### HTTPie

```sh
echo '{
	...claims
}' | http post \
	'{registry-url}/api/v1/{entity-type}/invite' \
	'content-type: application/json'
```
