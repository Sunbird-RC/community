# Get All Claims

{% swagger method="get" path="/api/v1/{entity-name}/claims" baseUrl=" " summary="" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-name" required="true" %}
Name of the entity to which attestation request was sent
{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to `application/json`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" %}
Set to `Bearer {access-token}` if `roles` in schema config is not anonymous else this can be empty
{% endswagger-parameter %}

{% swagger-parameter in="query" name="page" %}
Page no if you want response in pagenated format
{% endswagger-parameter %}

{% swagger-parameter in="query" name="size" %}
Maximum claims claims to be returned in one page
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    "totalPages": "",
    "content": [
        {
            "id": "{claimId}",
            "entity": "{entityName",
            "entityId": "{entityId}",
            "status": "OPEN",
            "attestationId": "{attestationID}",
            "attestationName": "{attestationName}",
            "closed": false
            ...
        }
    ]
    "totalElements": ""
}
```
{% endswagger-response %}
{% endswagger %}

### Usage

#### cURL

```shell
curl --location --request GET '{registry-url}/api/v1/{entity-name}/claims?page=1&size=10' \
--header 'Authorization: Bearer {access-token}'
```

#### HTTPie

```shell
http GET '{registry-url}/api/v1/{entity-name}/claims?page=1&size=10' \
 Authorization:'Bearer {access-token}' \
```

`{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`
