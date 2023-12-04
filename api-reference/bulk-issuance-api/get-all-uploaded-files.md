# Get all uploaded Files

{% swagger method="get" path="bulk/v1/uploads" baseUrl="/" summary="" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="false" %}
Set to Bearer {access-token} if roles in schema is not anonymous. Else authorization can be empty
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="With all the files that were uploaded" %}

{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="if the token is expired or you do not have appropriate permission to get uploaded files" %}

{% endswagger-response %}
{% endswagger %}
