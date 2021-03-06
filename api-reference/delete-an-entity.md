# Delete An Entity

To delete an entity, we need to make the following HTTP request:

## Request

```http
DELETE /api/v1/{entity-type}/{entity-id}
```

| Field           | In     | Type     | Description                    |
| --------------- | ------ | -------- | ------------------------------ |
| `content-type`  | header | `string` | Set to `application/json`      |
| `authorization` | header | `string` | Set to `bearer {access-token}` |
| `entity-type`   | path   | `string` | The type of entity to modify   |
| `entity-id`     | id     | `string` | The ID of entity to modify     |

## Response

This will delete the entity in the registry and return a blank HTTP 200 response.
## Usage

**cURL**

```sh
curl --location \
	 --request 'DELETE' \
	 --header 'content-type: application/json' \
	 --header 'authorization: bearer {access-token}' \
	 'http://localhost:8081/api/v1/Teacher/{id}'
```

**HTTPie**

```sh
http DELETE \
	'http://localhost:8081/api/v1/Teacher/{id}' \
	'content-type: application/json' \
	'authorization: bearer {access-token}'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found
> under the `rg` section in the `docker-compose.yaml` file and is usually
> `8081`.
