# Get Uploaded File

{% swagger method="get" path="/api/v1/{entity-type}/{entity-id}/{property}/documents/{document-id}" baseUrl=" " summary="" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-type" required="true" %}
Entity for which file was uploaded
{% endswagger-parameter %}

{% swagger-parameter in="path" required="true" name="entity-id" %}
Entity ID of corresponding entity
{% endswagger-parameter %}

{% swagger-parameter in="path" name="property" required="true" %}
any String for eg templates for uploading html template
{% endswagger-parameter %}

{% swagger-parameter in="path" name="document-id" required="true" %}
ID of document that was uploaded
{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to `application/octet-stream`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" required="true" %}
Set to `Bearer {access-token}` if `roles` in schema config is not anonymous else this can be empty
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success response of uploaded file" %}
```javascript
{file contents that was uploaded}
```
{% endswagger-response %}
{% endswagger %}



## Usage

### cURL

```shell
curl --location \
    --request GET '{registry-url}/api/v1/{entity-type/{entity-id}/{property}/documents/{document-id}' \
    --header 'Authorization: Bearer {access-token}'
```

### HTTPie

```shell
http '{registry-url}/api/v1/{entity-type/{entity-id}/{property}/documents/{document-id}' \
 Authorization:'Bearer {access-token}'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`.

