---
description: To update an entity, we need to make the following HTTP request
---

# Update An Entity

{% swagger method="put" path="/api/v1/{entity-type}/{id}" baseUrl=" " summary="Updating an Entity" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-type" required="true" %}
The type of entity to modify
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" required="true" %}
The ID of entity to modify
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" %}
Set to Bearer {access-token} if roles in schema is not anonymous. Else token can be empty
{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to `application/json`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="..." required="true" %}
The entity's data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Updated Entity Success Response" %}
```javascript
{
	"id": "sunbird-rc.registry.update",
	"ver": "1.0",
	"ets": 1634371946769,
	"params": {
		"resmsgid": "",
		"msgid": "d51e6e6a-027d-4a42-84bb-2ce00e31d993",
		"err": "",
		"status": "SUCCESSFUL",
		"errmsg": ""
	},
	"responseCode": "OK"
}
```
{% endswagger-response %}
{% endswagger %}



## Usage

### cURL

```
curl --location \
	--request 'PUT' \
	--header 'content-type: application/json' \
	--header 'authorization: bearer {access-token}' \
	--data-raw '{
		"phoneNumber": "1234567891",
		"school": "UP Public School",
		"subject": "Math",
		"name": "Pranav Agate",
	}' \
	'{registry-url}/api/v1/{entity-type}/{id}'
```

### HTTPie

```
printf '{
	"phoneNumber": "1234567891",
	"school": "UP Public School",
	"subject": "Math",
	"name": "Pranav Agate",
}' | http put \
	'{registry-url}/api/v1/{entity-type}/{id}' \
	'content-type: application/json' \
	'authorization: bearer {access-token}'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`.
