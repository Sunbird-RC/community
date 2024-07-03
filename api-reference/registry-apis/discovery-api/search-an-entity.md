# Search An Entity

<mark style="color:green;">`POST`</mark> `/api/v1/{entity-name}/search`

Or <mark style="color:blue;">`GET`</mark> `/api/v1/{entity-name}/search?search=<base64 encoded payload>`

#### Path Parameters

| Name                                          | Type   | Description                       |
| --------------------------------------------- | ------ | --------------------------------- |
| entity-name<mark style="color:red;">\*</mark> | String | Name of the entity to be searched |

#### Headers

| Name                                           | Type   | Description               |
| ---------------------------------------------- | ------ | ------------------------- |
| content-type<mark style="color:red;">\*</mark> | String | Set to `application/json` |

#### Query Parameters

| Name   | Type                   | Description                                                                                                                                                                                                                            |
| ------ | ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| search | base64 encoded payload | <p>Sets the query for searching and pagination of the data.<br>example: <br><code>eyJvZmZzZXQiOjIsImxpbWl0IjoyLCJmaWx0ZXJzIjp7fSwiZW50aXR5VHlwZSI6WyJJbnN1cmFuY2UiXX0=</code> contains { "offset": 2, "limit": 2, "filters": { } }</p> |

#### Request Body

| Name                                  | Type   | Description                                       |
| ------------------------------------- | ------ | ------------------------------------------------- |
| ...<mark style="color:red;">\*</mark> | Object | Filters to be sent in order to identify an entity |

{% tabs %}
{% tab title="200: OK Success Response of Search" %}
```javascript
{
    "totalCount": 10,
    "nextPage": "<registry-url>/api/v1/Student?search=<base64 encoded search payload>",
    "prevPage": "<registry-url>/api/v1/Student?search=<base64 encoded search payload>",
    "data": [
        {
            "school": "UP Public School",
            "name": "Pranav Agate",
            "contact": "1234567890",
            "subject": "Math",
            "osid": "{id}",
            "osOwner": ["{osOwner}"]
        }
    ]
}
```
{% endtab %}
{% endtabs %}

Sample Request Body

> {\
> "filters": {\
> "school": {\
> "eq": "UP Public School"\
> }\
> },\
> "limit": 1,\
> "offset": 0\
> }

> Important Fields in Request Body

| Field            | Type   | Description                                                                                                                                                             |
| ---------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| limit            | Number | Number of responses to be retrieved out of total number of search results                                                                                               |
| offset           | Number | Starting point for responses to be returned                                                                                                                             |
| viewTemplateId   | String | Any operation to be carried on the response before it is returned via API, that can be provided here. For eg: Combining First Name and Last Name into Single Field Name |
| Filter Operators | String | eq, neq, gte, gt, lte, lt, contains, notContains, between, or, startsWith, notStartsWith, endsWith, notEndsWith, queryString are some of the operators provided         |

### Usage

### cURL

```
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

```
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

> `{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`.
