# Publish A Schema

## Request

{% swagger method="put" path="/api/v1/Schema/{id}" baseUrl=" " summary="Publish a Schema" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to 

`application/json`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" required="false" %}
Set to 

`Bearer {access-token}`

 . The token should be a admin token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="status" type="" required="true" %}
PUBLISHED
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" required="true" %}
id of the schema that is to be published
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success Response of Published Schema" %}
```javascript
{
    "id": "sunbird-rc.registry.update",
    "ver": "1.0",
    "ets": 1669117705369,
    "params": {
        "resmsgid": "",
        "msgid": "2ff44354-bb9a-4dce-93ba-053f97587df6",
        "err": "",
        "status": "SUCCESSFUL",
        "errmsg": ""
    },
    "responseCode": "OK"
}
```
{% endswagger-response %}
{% endswagger %}

Sample Schema Request Payload

```json
{
  "status": "PUBLISHED"
}
```

### Usage

#### cURL

```shell
curl --location --request PUT '{registry-url}/api/v1/Schema/{id}' \
--header 'Authorization: Bearer {access-token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "status": "PUBLISHED"
}'
```

#### HTTPie

```
printf '{
  "status": "PUBLISHED"
}'| http  PUT '{registry-url}/api/v1/Schema' \
 Authorization:'Bearer {access-token}' \
 Content-Type:'application/json'
```

`{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`
