# Create An Entity

To create an entity, we need to make the following HTTP request:

## Request

```http
POST /api/v1/{entity-type}
```

| Field           | In     | Type     | Description                    |
| --------------- | ------ | -------- | ------------------------------ |
| `content-type`  | header | `string` | Set to `application/json`      |
| `authorization` | header | `string` | Set to `bearer {access-token}` |
| `entity-type`   | path   | `string` | The type of entity to create   |
| `mode`          | query  | `string` | Optional query parameter whose value can be `async` if creating an entity should be asynchronously handled
| `...`           | body   | `object` | The entity's data              |

> Sample Request Body <br>
{<br>
&emsp;"name": "Sunbird",<br>
&emsp;"school": "UP Public School",<br>
&emsp;"phoneNumber": "1234567890",<br>
&emsp;"subject": "Math"<br>
}


## Response

This will store the entity in the registry and return the following object:

```json
{
	"id": "sunbird-rc.registry.create",
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
	"result": { "{entityName}": { "osid": "1-9d6099fc-2c01-4714-bceb-55ff28c482f9" } }
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
		{
            "name": "Pranav Agate",
            "school": "UP Public School",
            "subject": "Math",
            "contact": "1234567890"
        }
	}' \
	'{registry-url}/api/v1/{entity-type}/'
```

### HTTPie

```sh
printf '{
            "name": "Pranav Agate",
            "school": "UP Public School",
            "subject": "Math",
            "contact": "1234567890"
        }' | http post \
	'{registry-url}/api/v1/{entity-type}' \
	'content-type: application/json'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found
> under the `registry` section in the `docker-compose.yml` file and is usually
> `8081`.
