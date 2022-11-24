# Delete A File/ Multiple Files

{% swagger method="delete" path="/api/v1/{entity-type}/{entity-id}/{property}/documents/{document-id}" baseUrl=" " summary="" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-type" required="true" %}
Entity type for which the template will be uploaded
{% endswagger-parameter %}

{% swagger-parameter in="path" name="entity-id" required="true" %}
Entity ID for mentioned Entity Type
{% endswagger-parameter %}

{% swagger-parameter in="path" name="property" required="true" %}
any String for eg templates for uploading html template
{% endswagger-parameter %}

{% swagger-parameter in="path" name="document-id" required="false" %}
ID of document that was uploaded. If this is not provided, entire directory that holds any no of files will be deleted
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" %}
Set to 

`Bearer {access-token}`

 if roles in schema is not anonymous. Else authorization can be empty
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success in deleting a/multiple files" %}
```javascript
```
{% endswagger-response %}
{% endswagger %}

### Usage

### cURL

```shell
curl --location \
    --request DELETE '{registry-url}/api/v1/{entity-type/{entity-id}/{property}/documents/{document-id}' \
    --header 'Authorization: Bearer {access-token}'
```

### HTTPie

```shell
http DELETE '{registry-url}/api/v1/{entity-type/{entity-id}/{property}/documents/{document-id}' \
 Authorization:'Bearer {access-token}'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`.
