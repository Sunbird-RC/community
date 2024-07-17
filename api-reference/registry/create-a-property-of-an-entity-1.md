---
description: >-
  To revoke an existing VC (Verifiable Credential) or an entity, we need to make
  the following HTTP request
---

# Revoke a Credential

## This revokes an existing  verifiable credential

<mark style="color:green;">`POST`</mark> `/api/v1/{entity-type}/{id}/revoke`

The API revokes a verifiable credential by updating the signature data attached to it. This is done by updating the _\_osSignedData_ field in the corresponding entity table to an _Empty String_. and storing the signedData in the **RevokedCredential** Registry.

#### Path Parameters

| Name                                          | Type   | Description                |
| --------------------------------------------- | ------ | -------------------------- |
| entity-type<mark style="color:red;">\*</mark> | String | Type of entity to update   |
| id<mark style="color:red;">\*</mark>          | String | id of the entity to revoke |

#### Headers

| Name                                           | Type   | Description                                                                                 |
| ---------------------------------------------- | ------ | ------------------------------------------------------------------------------------------- |
| authorization                                  | String | Set to `Bearer {access-token}` if roles in Schema is not anonymous. Else token can be empty |
| content-type<mark style="color:red;">\*</mark> | String | Set to `application/json`                                                                   |

#### Request Body

| Name | Type   | Description |
| ---- | ------ | ----------- |
|      | String |             |

{% tabs %}
{% tab title="200: OK Entity is Succesfully revoked" %}
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
{% endtab %}

{% tab title="401: Unauthorized Issue in the token authentication - unauthorised" %}

{% endtab %}

{% tab title="500: Internal Server Error Credential is already revoked " %}
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
{% endtab %}
{% endtabs %}

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
