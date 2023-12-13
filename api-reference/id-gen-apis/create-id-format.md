# Create ID Format



{% swagger method="post" path="" baseUrl="/id/_format/add" summary="Create Id format" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to `application/json`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RequestInfo" type="Object" required="true" %}
Parameters for logging
{% endswagger-parameter %}

{% swagger-parameter in="body" name="idRequests" type="Array" required="true" %}
&#x20;Object for Id Generation
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="Success response on format Creation" %}
```
{
  "responseInfo": {
    "apiId": "string",
    "ver": "string",
    "ts": 0,
    "resMsgId": "string",
    "msgId": "string",
    "status": "SUCCESSFUL"
  },
  "idResponses": [
    {
      "id": "string"
    }
  ]
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Invalid input" %}
```
{
  "requestInfo": {
    "apiId": "string",
    "ver": "string",
    "ts": 0,
    "msgId": "string"
  },
  "idRequests": [
    {
      "idName": "string",
      "tenantId": "string",
      "format": "string"
    }
  ]
}
```
{% endswagger-response %}
{% endswagger %}

Sample Request Object

```
{
  "requestInfo": {
    "apiId": "string",
    "ver": "string",
    "ts": 0,
    "msgId": "string"
  },
  "idRequests": [
    {
      "idName": "string",
      "tenantId": "string",
      "format": "string"
    }
  ]
}
```

## Usage

#### cURL

```
curl --location '{id-gen-url}/egov-idgen/id/_format/add' \
--header 'accept: application/json' \
--header 'Content-Type: application/json' \
--data '{
  "RequestInfo": {
    "apiId": "123",
    "ver": "12",
    "ts": 0,
    "msgId": "test123"
  },
  "idRequests": [
    {
      "idName": "sunbird.rc.Pledge",
      "tenantId": "s.rc.Pledge",
      "format": "D/[f:yyyyy-yy][SEQ_D_SRC_NUM]"
    }
  ]
}'
```

`{id-gen-url}` is usually http://localhost:{port}. The port can be found under the `id-gen-service` section in the `docker-compose.yml` file and is usually `8088`
