# Upload CSV

{% swagger method="post" path="/bulk/v1/uploadFiles/{schemaName}" baseUrl=" " summary="" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="schemaName" required="true" %}
Schema for which you want to bulk create entities
{% endswagger-parameter %}

{% swagger-parameter in="body" name="file" type="multipart/form-data" required="true" %}
csv file
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" %}
Set to Bearer {access-token} if roles in schema is not anonymous. Else authorization can be empty
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Information with ID of the file uploaded, number of success and failures" %}

{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="if the token is expired or you do not have appropriate permission to create entity" %}

{% endswagger-response %}

{% swagger-response status="500: Internal Server Error" description="If invalid csv file is passed" %}

{% endswagger-response %}
{% endswagger %}
