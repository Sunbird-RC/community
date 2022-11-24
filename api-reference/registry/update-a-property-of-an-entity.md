---
description: >-
  To update the property of an existing entity, we need to make the following
  HTTP request
---

# Update A Property Of An Entity

{% swagger method="put" path="/api/v1/{entity-type}/{id}/{entity-property}/{property-id}" baseUrl=" " summary="Update a property which is already added to entity" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-type" required="true" %}
Type of entity to update
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" required="true" %}
id of the entity to update
{% endswagger-parameter %}

{% swagger-parameter in="path" name="entity-property" required="true" %}
entity Property which to be updated in already existing entity
{% endswagger-parameter %}

{% swagger-parameter in="path" name="property-id" required="true" %}
entity property osid which is to be updated in already existing property in entity
{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to 

`application/json`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" %}
Set to 

`Bearer {access-token}`

 if roles in Schema is not anonymous. Else token can be empty
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "id": "sunbird-rc.registry.update",
    "ver": "1.0",
    "ets": 1669113253859,
    "params": {
        "resmsgid": "",
        "msgid": "129c291e-f089-4052-811f-025330c9b239",
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
	--request 'POST' \
	--header 'content-type: application/json' \
	--header 'authorization: bearer {access-token}' \
	--data-raw '{
		"city": "Surat"
	}' \
	'{registry-url}/api/v1/{entity-type}/{id}/{entity-property}/{property-id}'
```

### HTTPie

```
printf '{
		"city": "Surat"
	}' | http POST \
	'/api/v1/{entity-type}/{id}/{entity-property}/{property-id}' \
	'content-type: application/json' \
	'authorization: Bearer {access-token}'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`.
