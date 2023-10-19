# Revoke A Credential

{% swagger method="post" path="" baseUrl="" summary="Revoke an existing credential " %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-type" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="path" name="entity-id" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to `application/json`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" %}
Set to Bearer {access-token} if roles in Schema is not anonymous. Else token can be empty
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Credential Revoked Success Response" %}

{% endswagger-response %}
{% endswagger %}

{% code title="CURL" %}
```
curl --location --globoff --request POST '{registry-url}/api/v1///revoke'
--header 'content-type: application/json'
--header 'authorization: Bearer {access-token}'
```
{% endcode %}
