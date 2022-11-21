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
> Sample Request Body <br> 
{<br> &emsp;
	"entityName": "Teacher",<br> &emsp;
	"entityId": "{id}", <br> &emsp;
	"name": "schoolAffiliation"<br>
}<br>
> If you retrieve the entity by the [Retrieve Entity API Endpoint](./retrieving-an-entity.md), you can see the `id` field in `osid`

## Response

This will send the claim for attestation and return the following object:

```json
{
    "id": "sunbird-rc.registry.send",
    "ver": "1.0",
    "ets": 1668756344546,
    "params": {
        "resmsgid": "",
        "msgid": "d1ff3fa8-fac8-4b4b-a2a9-6910d395bfde",
        "err": "",
        "status": "SUCCESSFUL",
        "errmsg": ""
    },
    "responseCode": "OK",
    "result": {
        "attestationOSID": "1-c809dccf-4d93-453f-a110-d04443f5d679"
    }
}
```

If you retrieve the entity by the [Retrieve Entity API Endpoint](./retrieving-an-entity.md),
you will get the following object in response:

```json
{
	"schoolAffiliation": {
		"_osState": "ATTESTATION_REQUESTED",
		"osid": "{attestationOsId}",
		"userId": "{userId}",
		"_osClaimId": "{id}",
		"entityName": "Teacher",
		"entityId": "{id}",
		"name": "schoolAffiliation"
	}, 
	"osid": "{id}",
	"osOwner": ["{owner-id}"],
	"_osState/{field}": "ATTESTATION_REQUESTED"
}
```

Important variables in the response body:

| Field                | In   | Type     | Description                                                                                                                                                                                                                            |
| -------------------- | ---- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `osOwner`            | body | `string` | User ID of the entity in Keycloak.                                                                                                                                                                                                     |
| `{attestationName}/_osState`   | body | `string` | State of an attestable field. Can be `DRAFT` (when it has not been sent for attestation), `ATTESTATION_REQUESTED` (when sent for attestation), `PUBLISHED` (when successfully attested) and `REJECTED` (when rejected by the attestor) |
| `{affiliationName}/_osClaimId` | body | `string` | ID of the claim made on a field, required when attesting the claim.|
|`{affiliationName}/osid` | body | `string` | ID of the attestation that was requested

## Usage

### cURL

```sh
curl --location \
	--request 'POST' \
	--header 'content-type: application/json' \
	--header 'authorization: bearer {access-token}' \
	--data-raw '{"entityName": "Teacher",
		"entityId": "{id}",
		"name": "schoolAffiliation"}' \
	'{registry-url}/api/v1/send'
```

### HTTPie

```sh
printf '"entityName": "Teacher",
		"entityId": "{id}",
		"name": "schoolAffiliation"' | http POST \
	'{registry-url}/api/v1/send' \
	'content-type: application/json' \
	'authorization: bearer {access-token}'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found
> under the `registry` section in the `docker-compose.yml` file and is usually
> `8081`.
