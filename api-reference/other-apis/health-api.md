# Health API

{% swagger method="get" path="/health" baseUrl=" " summary="Return health of all services" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="Success of Health API" %}
```json
{
    "id": "sunbird-rc.registry.health",
    "ver": "1.0",
    "ets": 1669203912229,
    "params": {
        "resmsgid": "",
        "msgid": "e3deedb3-2c49-4d96-8d87-2b6f7b204e1c",
        "err": "",
        "status": "SUCCESSFUL",
        "errmsg": ""
    },
    "responseCode": "OK",
    "result": {
        "name": "sunbirdrc-registry-api",
        "healthy": false,
        "checks": [
            {
                "name": "sunbird.notification.service",
                "healthy": true,
                "err": "NOTIFICATION_ENABLED",
                "errmsg": "false"
            },
            {
                "name": "sunbird.encryption.service",
                "healthy": true,
                "err": "ENCRYPTION_ENABLED",
                "errmsg": "false"
            },
            {
                "name": "sunbird.signature.service",
                "healthy": true,
                "err": "",
                "errmsg": ""
            },
            {
                "name": "sunbird.certificate-api.service",
                "healthy": true,
                "err": "",
                "errmsg": ""
            },
            {
                "name": "sunbird.elastic.service",
                "healthy": true,
                "err": "",
                "errmsg": ""
            },
            {
                "name": "sunbird.keycloak.service",
                "healthy": true,
                "err": "",
                "errmsg": ""
            },
            {
                "name": "sunbird.file-storage.service",
                "healthy": false,
                "err": "CONNECTION_FAILURE",
                "errmsg": "The Access Key Id you provided does not exist in our records."
            }
        ]
    }
}
```
{% endswagger-response %}
{% endswagger %}

### Usage

#### cURL

```shell
curl --location --request GET 'http://localhost:8081/health'
```

#### HTTPie

```shell
http --follow --timeout 3600 GET 'http://localhost:8081/health'
```
