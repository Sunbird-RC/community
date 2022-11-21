# Search an Entity

To search for an entity, we need to make following request

## Request
```http
POST /api/v1/{entityName}/search
```
| Field            | In       | Type           | Description                                                         |
|----------------- | -------- | -------------- | ------------------------------------------------------------------- |
| `content-type`  | header | `string` | Set to `application/json`        |
| `entityName`    | path   | `string` | Name of the entity to be searched |
| `...`           | body   | `object` | Filters to be sent in order to identify an entity |

> Sample Request Body <br>
{<br>
    &emsp;"filters": {<br>
        &emsp;&emsp;"school": { <br>
            &emsp;&emsp;&emsp;"eq": "UP Public School" <br>
        &emsp;&emsp;} <br>
    &emsp;}, <br>
    &emsp;"limit": 1,<br>
    &emsp;"offset": 0<br>
}
>

## Response
This will return all the entities that matches the filter criteria

```json
[
    {
        "school": "UP Public School",
        "name": "Pranav Agate",
        "contact": "1234567890",
        "subject": "Math",
        "osid": "{id}",
        "osOwner": ["{osOwner}"]
    }
]
```

| Field              | In   | Type     | Description                                                                                                                                                   |
| ------------------ | ---- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `osid`             | body | string   | The ID of the create entity in the registry, used for retrieval and modification of the entity |


## Usage

### cURL

```sh
curl --location \
    --request POST \
    --header 'Content-Type: application/json' \
    '{registry-url}/api/v1/Place/search' \
    --data-raw '{
        "filters": {
            "name": {
                "eq": "UP Public School"
            }
        },
        "limit": 1,
        "offset": 0
    }'
```

### HTTPie

```sh
printf '{
    "filters": {
        "school": {
            "eq": "UP Public School"
        }
    },
    "limit": 1,
    "offset": 0
}'| http POST '{registry-url}/api/v1/Place/search' \
 Content-Type:'application/json' \
 ```


> `{registry-url}` is usually http://localhost:{port}. The port can be found
> under the `registry` section in the `docker-compose.yml` file and is usually
> `8081`.
