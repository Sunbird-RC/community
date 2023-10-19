# Get Schema

## Request

{% swagger method="get" path="/api/v1/Schema/{id}" baseUrl=" " summary="Fetch a Schema" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to `application/json`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" required="false" %}
Set to `Bearer {access-token}` . The token should be a admin token
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" required="true" %}
ID of Schema to be fetched
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success Response" %}
```javascript
{
    "schema": "...",
    "osUpdatedAt": "2022-11-22T10:30:27.602Z",
    "osCreatedAt": "2022-11-22T10:30:27.602Z",
    "osUpdatedBy": "0eaf6099-9591-42bb-ac5a-21bbc2b47493",
    "name": "schema",
    "osCreatedBy": "0eaf6099-9591-42bb-ac5a-21bbc2b47493",
    "osid": "1-1a2f15e7-6c54-40e9-a689-68628a3d69df",
    "osOwner": [
        "0eaf6099-9591-42bb-ac5a-21bbc2b47493"
    ],
    "status": "{schemaStatus}"
}
```
{% endswagger-response %}
{% endswagger %}

### Usage

#### cURL

```shell
curl --location --request GET '{registry-url}/api/v1/Schema/{id}' \
--header 'Authorization: Bearer {access-token}' \
--header 'Content-Type: application/json'
```

#### HTTPie

```
http  GET '{registry-url}/api/v1/Schema/{id}' \
 Authorization:'Bearer {access-token}' \
 Content-Type:'application/json'
```

`{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`
