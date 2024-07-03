---
description: >-
  To get an entity created by the owner, we need to make the following HTTP
  request
---

# Get An Entity

<mark style="color:blue;">`GET`</mark> `/api/v1/{entity-type}`

#### Path Parameters

| Name                                          | Type   | Description                    |
| --------------------------------------------- | ------ | ------------------------------ |
| entity-type<mark style="color:red;">\*</mark> | String | The type of entity to retrieve |

#### Query Parameters

Use search query parameter for pagination and filters

| Name   | Type                       | Description                                                                                                                                                                                                                            |
| ------ | -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| search | base64 encoded data string | <p>Sets the query for searching and pagination of the data.<br>example: <br><code>eyJvZmZzZXQiOjIsImxpbWl0IjoyLCJmaWx0ZXJzIjp7fSwiZW50aXR5VHlwZSI6WyJJbnN1cmFuY2UiXX0=</code> contains { "offset": 2, "limit": 2, "filters": { } }</p> |

#### Headers

| Name                                            | Type   | Description                                                           |
| ----------------------------------------------- | ------ | --------------------------------------------------------------------- |
| authorization<mark style="color:red;">\*</mark> | String | Set to `Bearer {access-token}` A valid token of the owner is required |
| viewTemplateId                                  | String | File name of view templates configured                                |

{% tabs %}
{% tab title="200: OK Response when content-type is application/json" %}
{% code overflow="wrap" %}
```javascript
{
	"totalCount": 1,
	"nextPage": "<registry-url>/api/v1/Student?search=<base64 encoded search payload>"
	"prevPage": "<registry-url>/api/v1/Student?search=<base64 encoded search payload>"
	"data": [{
		"phoneNumber": "1234567890",
		"school": "UP Public School",
		"subject": "Math",
		"name": "Pranav Agate",
		"osid": "{id}",
		"osOwner": ["{owner-id}"],
		"_osState/school": "DRAFT"
	}]
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
