---
description: To create an entity, we need to make the following HTTP request
---

# Create An Entity





## Request

{% swagger method="post" path="/api/v1/{entity-type}" baseUrl=" " summary="Create a record for entity-type" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-type" required="true" %}
The type of entity to create
{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to 

`application/json`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" required="false" %}
Set to 

`Bearer {access-token}`

 if 

`roles`

 in schema config is not anonymous else this can be empty
{% endswagger-parameter %}

{% swagger-parameter in="query" name="mode" %}
Query parameter whose value can be 

`async`

 if creating an entity should be asynchronously handled
{% endswagger-parameter %}

{% swagger-parameter in="body" name="..." type="" required="true" %}
The entity's data
{% endswagger-parameter %}

{% swagger-parameter in="query" name="callbackUrl" %}
Query parameter whose value will be a 

`web-hook url`

. The webook will be called once the entity is created in the registry. This is applicable only for 

`async mode`
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success Response of entity Created" %}
```javascript
{
	"id": "sunbird-rc.registry.create",
	"ver": "1.0",
	"ets": 1634198998956,
	"params": {
		"resmsgid": "",
		"msgid": "3ee6a76f-d6c8-4262-a7ee-ddbe66fcb127",
		"err": "",
		"status": "SUCCESSFUL",
		"errmsg": ""
	},
	"responseCode": "OK",
	"result": { "{entityName}": { "osid": "1-9d6099fc-2c01-4714-bceb-55ff28c482f9" } }
}
```
{% endswagger-response %}
{% endswagger %}

Sample Request Body for Teacher as Entity-Type

```json
{
 "name": "Sunbird",
 "school": "UP Public School",
 "phoneNumber": "1234567890",
 "subject": "Math"
}
```

Important Fields in Response Body

<table><thead><tr><th width="261.3333333333333">Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>result.{entity-type}.osid</code></td><td><code>string</code></td><td>The ID of the create entity in the registry, used for retrieval and modification of the entity</td></tr></tbody></table>

### Usage

So to create a `Teacher` entity named Pranav Agate who teaches Math at UP Public School, we would make the following API call:

#### cURL

```shell
curl --location \
	--request 'POST' \
	--header 'content-type: application/json' \
	--data-raw '{
		{
            "name": "Pranav Agate",
            "school": "UP Public School",
            "subject": "Math",
            "contact": "1234567890"
        }
	}' \
	'{registry-url}/api/v1/{entity-type}/'
```

#### HTTPie

```shell
printf '{
            "name": "Pranav Agate",
            "school": "UP Public School",
            "subject": "Math",
            "contact": "1234567890"
        }' | http post \
	'{registry-url}/api/v1/{entity-type}' \
	'content-type: application/json'
```

`{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`
