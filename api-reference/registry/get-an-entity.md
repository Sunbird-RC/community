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

{% swagger-parameter in="path" name="id" required="true" %}
The ID of the entity to retrieve
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" required="true" %}
Set to 

`Bearer {access-token}`

 A valid token of the owner is required
{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to 

`application/json`

 to retrieve in json. Other allowed values include 

`application/vc+ld+json`

, 

`application/ld+json`

, 

`application/pdf`

, 

`image/svg+xml`

, 

`text/html`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="template-key" %}
`template-key`

 is an optional header, it can be used for 

`pdf/html/svg`

 content type. It should be one of the keys mentioned in certificateTemplates in the schema config.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="template" %}
`template`

 is an optional header where we can pass the URL of the external template directly in the API. To use this 

`enable_external_templates`

 ENV needs to be enabled
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
