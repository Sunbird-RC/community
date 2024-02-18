# Search An Entity

{% swagger method="post" path="/api/v1/{entity-name}/search" baseUrl=" " summary="" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-name" required="true" %}
Name of the entity to be searched
{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to `application/json`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="..." type="Object" required="true" %}
Filters to be sent in order to identify an entity
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success Response of Search" %}
```javascript
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
{% endswagger-response %}
{% endswagger %}

Sample Request Body

> {\
>  "filters": {\
>   "school": {\
>    "eq": "UP Public School"\
>   }\
>  },\
>  "limit": 1,\
>  "offset": 0\
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
