---
description: >-
  To revoke an existing VC (Verifiable Credential), we need to make the
  following HTTP request
---

# Revoke An Entity

{% swagger method="post" path="/api/v1/{entity-type}/{id}/revoke" baseUrl=" " summary="This revokes an existing  verifiable credential" expanded="true" %}
{% swagger-description %}


The API revokes a verifiable credential by updating the signature data attached to it. This is done by updating the _\_osSignedData_ field in the corresponding entity table to an _Empty String_. and storing the signedData in the **RevokedCredential** Registry.
{% endswagger-description %}

{% swagger-parameter in="path" name="entity-type" required="true" %}
Type of entity to update
{% endswagger-parameter %}

{% swagger-parameter in="path" required="true" name="id" %}
id of the entity to revoke
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" %}
Set to `Bearer {access-token}` if roles in Schema is not anonymous. Else token can be empty
{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to `application/json`
{% endswagger-parameter %}

{% swagger-parameter in="body" %}

{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Entity is Succesfully revoked" %}
```json
{
    "id": "sunbird-rc.utils.revoke",
    "ver": "1.0",
    "ets": 1687421017075,
    "params": {
        "resmsgid": "",
        "msgid": "bbd1e0b7-aab4-4e91-988b-c832e9031bef",
        "err": "",
        "status": "SUCCESSFUL",
        "errmsg": ""
    },
    "responseCode": "OK"
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Issue in the token authentication - unauthorised" %}

{% endswagger-response %}

{% swagger-response status="500: Internal Server Error" description="Credential is already revoked " %}


```json
revoked response: {
    "id": "sunbird-rc.utils.revoke",
    "ver": "1.0",
    "ets": 1687421091703,
    "params": {
        "resmsgid": "",
        "msgid": "70047a46-a0f1-453a-b6cf-c1cfe8e1a544",
        "err": "",
        "status": "UNSUCCESSFUL",
        "errmsg": "Credential is already revoked"
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
	--request POST \
	--header 'Content-Type: application/json' \
	--header 'Accept: application/json' \
	'{registry-url}/api/v1/{entity-type}/{id}/revoke' \


```

### HTTPie

```
http POST \
	'{registry_url}/api/v1/{entity-type}/{id}/revoke' \
	'content-type: application/json' \
	'authorization: Bearer {access-token}'
	
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`.
