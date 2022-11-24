# Certificate API

Downloading PDF version of certificate

{% swagger method="get" path="EntityNames/Id" baseUrl="/api/v1/" summary="Download pdf certificate" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" type="Accept" required="true" %}
application/pdf
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="PDF Copy of the certificate" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}
