---
description: To invite an entity, we need to make the following HTTP request
---

# Invite An Entity

{% swagger method="post" path="/api/v1/{entity-type}/invite" baseUrl=" " summary="Invite an Entity" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to 

`application/json`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" %}
Set to 

`Bearer {access-token}`

 if 

`inviteRoles`

 in schema config is not anonymous else this can be empty
{% endswagger-parameter %}

{% swagger-parameter in="path" name="entity-type" required="true" %}
The type of entity to create
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="..." type="Object" %}
The entity's data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success response of entity invited" %}
```json
{
	"id": "sunbird-rc.registry.invite",
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
	"result": { "Teacher": { "osid": "1-9d6099fc-2c01-4714-bceb-55ff28c482f9" } }
}
```
{% endswagger-response %}
{% endswagger %}

Important Fields in Response Body

| Field                       | Type     | Description                                                                                    |
| --------------------------- | -------- | ---------------------------------------------------------------------------------------------- |
| `result.{entity-type}.osid` | `string` | The ID of the create entity in the registry, used for retrieval and modification of the entity |

## Usage

So to create a `Teacher` entity named Pranav Agate who teaches Math at UP Public School, we would make the following API call:

### cURL

```
curl --location \
	--request 'POST' \
	--header 'content-type: application/json' \
	--data-raw '{
		"phoneNumber": "1234567890",
		"school": "UP Public School",
		"subject": "Math",
		"name": "Pranav Agate",
	}' \
	'{registry-url}/api/v1/Teacher/invite'
```

### HTTPie

```
printf '{
    "name": "Pranav Agate",
    "teaches": "Math",
    "school": "UP Public School"
}'| http POST '{registry-url}/api/v1/Teacher/invite' \
 Content-Type:'application/json'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`.

#### Note: Invite API doesn't validate the required parameters. Invite API is designed to be used to invite another actor to the system with minimal information, hence required validations will not be applied. Instead, use [create an entity API](create-an-entity.md).
