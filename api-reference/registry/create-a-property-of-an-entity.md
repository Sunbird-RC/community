---
description: >-
  To add property to an existing entity, we need to make the following HTTP
  request
---

# Create A Property Of An Entity

{% swagger method="post" path="/api/v1/{entity-type}/{id}/{entity-property}" baseUrl=" " summary="Create a property in already existing entity" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-type" required="true" %}
Type of entity to update
{% endswagger-parameter %}

{% swagger-parameter in="path" required="true" name="id" %}
id of the entity to update
{% endswagger-parameter %}

{% swagger-parameter in="path" name="entity-property" required="true" %}
entity Property which to be added in already existing entity
{% endswagger-parameter %}

{% swagger-parameter in="body" name="..." type="Object" required="true" %}
Property to be updated
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" %}
Set to 

`Bearer {access-token}`

 if roles in Schema is not anonymous. Else token can be empty
{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to 

`application/json`
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "id": "sunbird-rc.registry.update",
    "ver": "1.0",
    "ets": 1669113170690,
    "params": {
        "resmsgid": "",
        "msgid": "85057df5-d4d6-4e7b-8bb1-a774f689e9c8",
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
		"city": "Ahmedabad"
	}' \
	'{registry-url}/api/v1/{entity-type}/{id}/{entity-property}'
```

### HTTPie

```
printf '{
		"city": "Ahmedabad"
	}' | http POST \
	'{registry-url}/api/v1/{entity-type}/{id}/{entity-property}' \
	'content-type: application/json' \
	'authorization: Bearer {access-token}'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`.
