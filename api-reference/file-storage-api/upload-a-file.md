# Upload A File

{% swagger method="post" path="/api/v1/{entity-type}/{entity-id}/{property}/documents" baseUrl=" " summary="" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-type" required="true" %}
Entity type for which the template will be uploaded
{% endswagger-parameter %}

{% swagger-parameter in="path" name="entity-id" required="true" %}
Entity ID for mentioned Entity Type
{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to 

`multipart/form-data`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" %}
Set to 

`Bearer {access-token}`

 if 

`roles`

 in schema config is not anonymous else this can be empty
{% endswagger-parameter %}

{% swagger-parameter in="body" name="files" type="Object" required="true" %}
`files`

 is template-key which can be replaced with your choice. Requires a 

`html file`

 to be sent which will be uploaded
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success Response of File uploaded" %}
```javascript
{
    "documentLocations": [
        "{url}"
    ],
    "errors": []
}
```
{% endswagger-response %}
{% endswagger %}



## Usage

### cURL

```
curl --location \
    --header 'Authorization: {access-token}' \
    --form 'files=@"{file-path}"'
    --request POST 
    '{registry-url}/api/v1/{entity-type}/{entity-id}/{property}/documents' \
```

### HTTPie

```
http --ignore-stdin \
    --form POST \
    '{registry-url}/api/v1/{entity-type}/{entity-id}/{property}/documents' \
 'files'@{file-path} \
 Authorization:'Bearer {access-token}' \
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`.
