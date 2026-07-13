# Raise An Attestation

## Making a Claim

<mark style="color:green;">`POST`</mark> `/api/v1/send`

#### Headers

| Name                                           | Type   | Description                                                                                                                              |
| ---------------------------------------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| content-type<mark style="color:red;">\*</mark> | String | Set to `application/json`                                                                                                                |
| authorization                                  | String | Set to `Bearer {access-token}` if roles in schema of which attestation is to raised does not contain `anonymous` else token can be empty |

#### Request Body

| Name                                  | Type   | Description            |
| ------------------------------------- | ------ | ---------------------- |
| ...<mark style="color:red;">\*</mark> | Object | The value of the claim |

{% tabs %}
{% tab title="200: OK Success Response of attestation sent" %}
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
{% endtab %}
{% endtabs %}

Sample Request Body

```json
{
  "entityName": "Teacher",
  "entityId": "{id}",
  "name": "schoolAffiliation" // attestation name
   "propertiesOSID": { // OSIDs of properties to be attested
   }
}
```

\
\
If you retrieve the entity by the [Retrieve Entity API Endpoint](https://github.com/Sunbird-RC/community/blob/v2.0.0/api-reference/attestation-api/broken-reference/README.md), you can see the `id` field in `osid`

### Usage

#### cURL

```shell
curl --location --request POST '{registry-url}/api/v1/send' \
--header 'Authorization: Bearer {access-token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "entityName": "Teacher",
  "entityId": "{id}",
  "name": "schoolAffiliation"
}'
```

#### HTTPie

```shell
printf '{
  "entityName": "Teacher",
  "entityId": "{id}",
  "name": "schoolAffiliation"
}'| http POST '{registry-url}/api/v1/send' \
 Authorization:'Bearer {access-token}' \
 Content-Type:'application/json' \
```

`{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`
