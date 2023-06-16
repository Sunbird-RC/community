# Get Count

{% swagger method="get" path="http://localhost:8070/v1/metrics" baseUrl=" " summary="" expanded="true" %}
{% swagger-description %}
Get count of events emitted for each entityType
{% endswagger-description %}

{% swagger-response status="200: OK" description="Retrieve all the events for each entityType based grouped on the basis of operationType. Eg: {schema: { add: 5, read: 2}}" %}

{% endswagger-response %}
{% endswagger %}

