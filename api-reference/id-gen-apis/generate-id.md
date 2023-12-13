# Generate ID

This service generates an ID/Code given the desired format e.g. an id with format AP-PT-2017/04/11-000001-23 where format is can be decomposed as (`-` is used as a separator in the above example) -

AP - Fixed string indicating the tenant

PT - fixed string indicating module

2017/04/11 - Date field indicating YYYY/MM/DD

000001 - local sequence number

23 - two random digits

This can be indicated to IDGen service as an ID needed in the format (square brackets indicate the parts that will be replaced by the svc to generate the new id) -\
AP-PT-\[YYYY/MM/DD]-\[SEQ\_ACK\_NUM]-\[d{2}]

{% swagger method="post" path="" baseUrl="/id/_generate" summary="API to generate new id based on the id formats passed." expanded="true" %}
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

{% swagger-response status="201: Created" description="Success response on Id generation" %}


```json
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


```json
{
  "ResponseInfo": {
    "apiId": "string",
    "ver": "string",
    "ts": 0,
    "resMsgId": "string",
    "msgId": "string",
    "status": "SUCCESSFUL"
  },
  "Errors": [
    {
      "code": "string",
      "message": "string",
      "description": "string",
      "params": [
        "string"
      ]
    }
  ],
  "codes": "InvalidIDFormat"
}
```
{% endswagger-response %}
{% endswagger %}

Sample Request Body

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



#### CuRL



```sh
curl --location '{id-gen-url}/egov-idgen/id/_generate' \
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
