# Download a Report File



{% swagger method="get" path="bulk/v1/{id}/report" baseUrl="/" summary="" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter name="id" in="path" required="true" %}
Id of the report file you want to download
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" required="false" %}
Set to Bearer {access-token} if roles in schema is not anonymous. Else authorization can be empty
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="File will be downloaded with error response" %}

{% endswagger-response %}
{% endswagger %}
