# Other APIs



{% swagger method="get" path="" baseUrl="{certificate-api}/verify" summary="Verify W3 verifiable credential api" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="signedCredentials" %}
Credential object
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="{certificate-signer}/api/v1/certificatePDF" summary="Create PDF template based on template" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="certificate" required="true" %}
JSON certificate body
{% endswagger-parameter %}

{% swagger-parameter in="body" name="templateUrl" %}
Template for the certificate
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="PDF response body" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}
