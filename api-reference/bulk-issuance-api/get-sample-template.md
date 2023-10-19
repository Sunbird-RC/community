# Get Sample Template

{% swagger method="get" path="/bulk/v1/sample/{schemaName}" baseUrl=" " summary="" %}
{% swagger-description %}
this will download a csv with the all fields that are needed to create entity for this schema
{% endswagger-description %}

{% swagger-parameter in="path" name="schemaName" type="String" required="true" %}
name of schema&#x20;
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" %}
Set to `Bearer {access-token}` if roles in schema is not anonymous. Else authorization can be empty
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="A CSV File with fields for header" %}

{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="if the token is expired or you do not have appropriate permission to create entity" %}

{% endswagger-response %}

{% swagger-response status="404: Not Found" description="If schema is not found in the system" %}

{% endswagger-response %}
{% endswagger %}
