# Attest/Reject A Claim

To attest/reject a claim, we need to make the following request:

## Request

```http
POST /api/v1/{attestor-entity-type}/claims/{claim-id}/attest
```

| Field                  | In     | Type     | Description                                                                     |
| ---------------------- | ------ | -------- | ------------------------------------------------------------------------------- |
| `content-type`         | header | `string` | Set to `application/json`                                                       |
| `attestor-entity-type` | path   | `string` | The attestor entity types                                                       |
| `claim-id`             | path   | `string` | The ID of the claim to attest                                                   |
| `action`               | body   | `string` | Set to `GRANT_CLAIM` to attest the claim and `REJECT_CLAIM` to reject the claim |
| `notes`                | body   | `string` | Attestation related details (optional)                                          |

## Response

This will attest/reject the claim and return a blank HTTP 200 response.

## Usage

### cURL

```sh
curl --location \
	--request 'POST' \
	--header 'content-type: application/json' \
	--header 'authorization: bearer {access-token}' \
	--data-raw '{
		"action": "GRANT_CLAIM"
	}' \
	'{registry-url}/api/v1/{attestor-entity-type}/claims/{claim-id}/attest'
```

### HTTPie

```sh
echo '{
	"action": "GRANT_CLAIM"
}' | http post \
	'{registry-url}/api/v1/{attestor-entity-type}/claims/{claim-id}/attest' \
	'content-type: application/json' \
	'authorization: bearer {access-token}'
```
