# Swagger JSON API

{% swagger method="get" path="/api/docs/swagger.json" baseUrl="" summary="Returns all available registry apis" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="Success Response" %}
```json
{
    "consumes": [
        "application/json"
    ],
    "produces": [
        "application/json"
    ],
    "schemes": [
        "https"
    ],
    "swagger": "2.0",
    "info": {
        "description": "Sunbird registry and credential api (SunbirdRC)",
        "title": "Sunbird Registry and Credential",
        "version": "1.0.0"
    },
    "host": "ndear.xiv.in",
    "basePath": "/",
    "paths": {
        ...
    },
    "definitions": {
        ...
    }
}
```
{% endswagger-response %}
{% endswagger %}
