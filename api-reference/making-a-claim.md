# Make A Claim

To make a claim and send it for attestation, we need to make the following
request:

## Request

```http
POST /api/v1/send
```

| Field           | In     | Type      | Description                                          |
| --------------- | ------ | --------- | ---------------------------------------------------- |
| `content-type`  | header | `string`  | Set to `application/json`                            |
| `authorization` | header | `string`  | Set to `Bearer {access-token}|                                      
| `...`           | body   | `any`     | The value of the claim                               |

## Response

This will send the claim for attestation and return the following object:

```json
{
	"id": "open-saber.registry.update",
	"ver": "1.0",
	"ets": 1634371946769,
	"params": {
		"resmsgid": "",
		"msgid": "ksi38Dsl-8dIw-492j-6vlS-84KRe0Csop35",
		"err": "",
		"status": "SUCCESSFUL",
		"errmsg": ""
	},
	"responseCode": "OK"
}
```

If you retrieve the entity by the [Retrieve Entity API Endpoint](./retrieving-an-entity.md),
you will get the following object in response:

```json
{
	...claims
	"osid": "{id}",
	"osOwner": ["{owner-id}"],
	"_osClaimId/{field}": "{claim-id}",
	"_osState/{field}": "ATTESTATION_REQUESTED"
}
```

Important variables in the response body:

| Field                | In   | Type     | Description                                                                                                                                                                                                                            |
| -------------------- | ---- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `osOwner`            | body | `string` | User ID of the entity in Keycloak.                                                                                                                                                                                                     |
| `_osState/{field}`   | body | `string` | State of an attestable field. Can be `DRAFT` (when it has not been sent for attestation), `ATTESTATION_REQUESTED` (when sent for attestation), `PUBLISHED` (when successfully attested) and `REJECTED` (when rejected by the attestor) |
| `_osClaimId/{field}` | body | `string` | ID of the claim made on a field, required when attesting the claim.                                                                                                                                                                    |

## Usage

### cURL

```sh
curl --location \
	--request 'POST' \
	--header 'content-type: application/json' \
	--header 'authorization: bearer {access-token}' \
	--data-raw '...field-value' \
	'{registry-url}/api/v1/{entity-type}/{id}/{field}?send=true'
```

### HTTPie

```sh
printf '...field-value' | http POST \
	'{registry-url}/api/v1/{entity-type}/{id}/{field}' \
	'content-type: application/json' \
	'authorization: bearer {access-token}' \
	'send==true'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found
> under the `rg` section in the `docker-compose.yaml` file and is usually
> `8081`.
