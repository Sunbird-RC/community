# Create Schema

## Request

{% swagger method="post" path="/api/v1/Schema" baseUrl=" " summary="Create a Schema" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to `application/json`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" required="false" %}
Set to `Bearer {access-token}` . The token should be a admin token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" type="string" required="true" %}
schema name
{% endswagger-parameter %}

{% swagger-parameter in="body" name="schema" type="string" required="true" %}
json schema
{% endswagger-parameter %}

{% swagger-parameter in="body" name="status" %}
DRAFT | PUBLISHED
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success Response of entity Created" %}
```javascript
{
    "id": "sunbird-rc.registry.create",
    "ver": "1.0",
    "ets": 1669113026569,
    "params": {
        "resmsgid": "",
        "msgid": "7b15f6ee-eb5b-4a13-ba79-f70b3ab54fc3",
        "err": "",
        "status": "SUCCESSFUL",
        "errmsg": ""
    },
    "responseCode": "OK",
    "result": {
        "Schema": {
            "osid": "1-1a2f15e7-6c54-40e9-a689-68628a3d69df"
        }
    }
}
```
{% endswagger-response %}
{% endswagger %}

Sample Schema Request Payload

```json
{
  "name": "{schema-name}",
  "schema": "{   \"$schema\": \"http://json-schema.org/draft-07/schema\",   \"type\": \"object\",   \"properties\": {      \"Place\": {         \"$ref\": \"#/definitions/Place\"      }   },   \"required\": [      \"Place\"   ],   \"title\": \"Place\",   \"definitions\": {      \"Place\": {         \"$id\": \"#/properties/Place\",         \"type\": \"object\",         \"title\": \"The Place Schema\",         \"required\": [            \"name\",            \"city\"],         \"properties\": {            \"name\": {               \"type\": \"string\"            },            \"city\": {               \"type\": \"string\"            }},   \"_osConfig\": {      \"privateFields\": [         \"name\"      ],      \"signedFields\": [],      \"roles\": [         \"anonymous\"      ],      \"ownershipAttributes\": [],      \"attestationPolicies\": [         {            \"name\": \"schemaAttestation\",            \"conditions\": \"(ATTESTOR#$.[*]#.contains('board-cbse'))\",            \"type\": \"MANUAL\",            \"attestorPlugin\": \"did:internal:ClaimPluginActor?entity=board-cbse\",            \"attestationProperties\": {}         }      ],      \"credentialTemplate\": {         \"@context\": [            \"https://www.w3.org/2018/credentials/v1\",            \"{someUrlForTemplate}\"         ],         \"type\": [            \"VerifiableCredential\",            \"{TypeOfCertificate}\"         ],         \"issuer\": \"{issuerName}\",         \"issuanceDate\": \"{issuanceDate}\",         \"credentialSubject\": {            \"type\": \"Place\",            \"name\": \"{name}\"},         \"evidence\": {            \"type\": \"{SomeType}\"}},      \"certificateTemplates\": {        \"html\": \"{templateUrl}\"      }   }}}}"
}
```

Important Fields in Response Body

| Field                  | Type     | Description                                                                                     |
| ---------------------- | -------- | ----------------------------------------------------------------------------------------------- |
| `result.{Schema}.osid` | `string` | The ID of the created schema in the registry, used for retrieval and modification of the entity |

### Usage

#### cURL

```shell
curl --location --request POST '{registry-url}/api/v1/Schema' \
--header 'Authorization: Bearer {access-token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "{schema-name}",
  "schema": "{   \"$schema\": \"http://json-schema.org/draft-07/schema\",   \"type\": \"object\",   \"properties\": {      \"Place\": {         \"$ref\": \"#/definitions/Place\"      }   },   \"required\": [      \"Place\"   ],   \"title\": \"Place\",   \"definitions\": {      \"Place\": {         \"$id\": \"#/properties/Place\",         \"type\": \"object\",         \"title\": \"The Place Schema\",         \"required\": [            \"name\",            \"city\"],         \"properties\": {            \"name\": {               \"type\": \"string\"            },            \"city\": {               \"type\": \"string\"            }},   \"_osConfig\": {      \"privateFields\": [         \"name\"      ],      \"signedFields\": [],      \"roles\": [         \"anonymous\"      ],      \"ownershipAttributes\": [],      \"attestationPolicies\": [         {            \"name\": \"schemaAttestation\",            \"conditions\": \"(ATTESTOR#$.[*]#.contains('board-cbse'))\",            \"type\": \"MANUAL\",            \"attestorPlugin\": \"did:internal:ClaimPluginActor?entity=board-cbse\",            \"attestationProperties\": {}         }      ],      \"credentialTemplate\": {         \"@context\": [            \"https://www.w3.org/2018/credentials/v1\",            \"{someUrlForTemplate}\"         ],         \"type\": [            \"VerifiableCredential\",            \"{TypeOfCertificate}\"         ],         \"issuer\": \"{issuerName}\",         \"issuanceDate\": \"{issuanceDate}\",         \"credentialSubject\": {            \"type\": \"Place\",            \"name\": \"{name}\"},         \"evidence\": {            \"type\": \"{SomeType}\"}},      \"certificateTemplates\": {        \"html\": \"{templateUrl}\"      }   }}}}"
}'
```

#### HTTPie

```
printf '{
  "name": "{schema-name}",
  "schema": "{   \"$schema\": \"http://json-schema.org/draft-07/schema\",   \"type\": \"object\",   \"properties\": {      \"Place\": {         \"$ref\": \"#/definitions/Place\"      }   },   \"required\": [      \"Place\"   ],   \"title\": \"Place\",   \"definitions\": {      \"Place\": {         \"$id\": \"#/properties/Place\",         \"type\": \"object\",         \"title\": \"The Place Schema\",         \"required\": [            \"name\",            \"city\"],         \"properties\": {            \"name\": {               \"type\": \"string\"            },            \"city\": {               \"type\": \"string\"            }},   \"_osConfig\": {      \"privateFields\": [         \"name\"      ],      \"signedFields\": [],      \"roles\": [         \"anonymous\"      ],      \"ownershipAttributes\": [],      \"attestationPolicies\": [         {            \"name\": \"schemaAttestation\",            \"conditions\": \"(ATTESTOR#$.[*]#.contains('board-cbse'))\",            \"type\": \"MANUAL\",            \"attestorPlugin\": \"did:internal:ClaimPluginActor?entity=board-cbse\",            \"attestationProperties\": {}         }      ],      \"credentialTemplate\": {         \"@context\": [            \"https://www.w3.org/2018/credentials/v1\",            \"{someUrlForTemplate}\"         ],         \"type\": [            \"VerifiableCredential\",            \"{TypeOfCertificate}\"         ],         \"issuer\": \"{issuerName}\",         \"issuanceDate\": \"{issuanceDate}\",         \"credentialSubject\": {            \"type\": \"Place\",            \"name\": \"{name}\"},         \"evidence\": {            \"type\": \"{SomeType}\"}},      \"certificateTemplates\": {        \"html\": \"{templateUrl}\"      }   }}}}"
}'| http  POST '{registry-url}/api/v1/Schema' \
 Authorization:'Bearer {access-token}' \
 Content-Type:'application/json'
```

`{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`
