---
description: To get an entity, we need to make the following HTTP request
---

# Get An Entity By Id

{% swagger method="get" path="/api/v1/{entity-type}/id" baseUrl=" " summary="" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-type" required="true" %}
The type of entity to retrieve
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" required="true" %}
The ID of the entity to retrieve
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" required="true" %}
Set to `Bearer {access-token}` if roles in schema is not anonymous. Else authorization can be empty
{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to `application/json` to retrieve in json. Other allowed values include `application/vc+ld+json`, `application/ld+json`, `application/pdf`, `image/svg+xml`, `text/html`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="template-key" %}
`template-key` is an optional header, it can be used for `pdf/html/svg` content type. It should be one of the keys mentioned in certificateTemplates in the schema config.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="template" %}
`template` is an optional header where we can pass the URL of the external template directly in the API. To use this `enable_external_templates` ENV needs to be enabled
{% endswagger-parameter %}

{% swagger-parameter in="header" name="viewTemplateId" %}
File name of view templates configured
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

Important variables in the response body:

| Field              | Type   | Description                                                                                                                                                                                                                            |
| ------------------ | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `_osState/{field}` | String | State of an attestable field. Can be `DRAFT` (when it has not been sent for attestation), `ATTESTATION_REQUESTED` (when sent for attestation), `PUBLISHED` (when successfully attested) and `REJECTED` (when rejected by the attestor) |
| \_osOwner          | String | User ID of the entity in Keycloak.                                                                                                                                                                                                     |
