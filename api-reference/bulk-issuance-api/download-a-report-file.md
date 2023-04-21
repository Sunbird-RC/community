# Download a Report File

{% swagger method="get" path="/bulk/v1/download/{id}" baseUrl="" summary="" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" required="true" %}
Id of the report file you want to download
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" %}
Set to Bearer {access-token} if roles in schema is not anonymous. Else authorization can be empty
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="File will be downloaded with error response" %}

{% endswagger-response %}
{% endswagger %}
