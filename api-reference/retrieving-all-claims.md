# Retrieve All Claims

To retrieve all claims, we need to make following request

## Request
```http
GET /api/v1/{entity-name}/claims
```

| Field            | In       | Type           | Description                                                         |
|----------------- | -------- | -------------- | ------------------------------------------------------------------- |
| `content-type`  | header | `string` | Set to `application/json`        |
| `authorization` | header | `string` | Set to `Bearer {access-token}`   |
| `entityName`     | path  | `string` | Name of the entity to which attestation request was sent|

## Response

This will return all the claims in JSON representation as follows:

```json
{
    "totalPages": "",
    "content": [
        {
            "id": "{claimId}",
            "entity": "{entityName",
            "entityId": "{entityId}",
            "status": "OPEN",
            "attestationId": "{attestationID}",
            "attestationName": "{attestationName}",
            "closed": false
            ...
        }
    ]
    "totalElements": ""
}
```
> status can be OPEN or CLOSED based on whether no action has been taken or it was attestated/rejected.

Important variables in Response Body

| Field              | In   | Type     | Description                                                                                                                                                   |
| ------------------ | ---- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `id`               | body | string   | ID of claim that was raised|
| `entityId`         | body | string   | osid of entity for which claim was raised|
| `status`           | body | string   | status can be `OPEN` or `CLOSED` |
|  `closed`          | body | string   | boolean value if this claim was `approved/rejected`

## Usage

### cURL

```sh
curl --location \
    --request GET \
    --header 'Authorization: Bearer {access-token}' \
    '{registry-url}/api/v1/board-cbse/claims' \
```

### HTTPie
```sh
http GET \
    '{registry-url}/api/v1/board-cbse/claims' \
    Authorization:'Bearer {access-token}' \
```


> `{registry-url}` is usually http://localhost:{port}. The port can be found
> under the `registry` section in the `docker-compose.yml` file and is usually
> `8081`.
