# Get Aggregates



{% swagger method="get" path=" " baseUrl="http://localhost:8070/v1/aggregates" summary="This API will emit the aggregates of events that occurred in the specified time period. Time period can be configured in the metrics service using configurations" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="Object with aggregates over the period of configured time. Eg {schema: {add: 3, delete: 1}}}" %}

{% endswagger-response %}
{% endswagger %}
