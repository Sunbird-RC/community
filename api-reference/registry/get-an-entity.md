---
description: >-
  To get an entity created by the owner, we need to make the following HTTP
  request
---

# Get An Entity



{% swagger method="get" path="/api/v1/{entity-type}" baseUrl=" " summary="" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-type" required="true" %}
The type of entity to retrieve
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" required="true" %}
Set to 

`Bearer {access-token}`

 A valid token of the owner is required
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Response when content-type is application/json" %}
```javascript
{
	"phoneNumber": "1234567890",
	"school": "UP Public School",
	"subject": "Math",
	"name": "Pranav Agate",
	"osid": "{id}",
	"osOwner": ["{owner-id}"],
	"_osState/school": "DRAFT"
}
```
{% endswagger-response %}
{% endswagger %}
