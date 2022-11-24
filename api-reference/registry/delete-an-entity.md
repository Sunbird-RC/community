---
description: To soft delete an entity, we need to make the following HTTP request
---

# Delete An Entity

{% swagger method="delete" path="/api/v1/{entity-type}/{id}" baseUrl=" " summary="Deleting an Entity" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-type" required="true" %}
The type of entity to modify
{% endswagger-parameter %}

{% swagger-parameter in="path" name="entity-id" required="true" %}
The ID of entity to modify
{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to 

`application/json`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" required="false" %}
Set to Bearer {access-token} if roles in Schema is not anonymous. Else token can be empty
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Entity Deleted Success Response" %}
```javascript
```
{% endswagger-response %}
{% endswagger %}



## Usage

**cURL**

```
curl --location \
	 --request 'DELETE' \
	 --header 'content-type: application/json' \
	 --header 'authorization: Bearer {access-token}' \
	 '{registry-url}/api/v1/Teacher/{id}'
```

**HTTPie**

```
http DELETE \
	'{registry_url}/api/v1/Teacher/{id}' \
	'content-type: application/json' \
	'authorization: Bearer {access-token}'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`.
