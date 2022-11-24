# Update Schema

## Request

{% swagger method="put" path="/api/v1/Schema/{id}" baseUrl=" " summary="Create a Schema" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to 

`application/json`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" required="false" %}
Set to 

`Bearer {access-token}`

 . The token should be a admin token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="..." type="" required="true" %}
The entity's data
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" required="true" %}
id of the schema that is to be updated
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success Response of Updated Schema" %}
```javascript
{
    "id": "sunbird-rc.registry.update",
    "ver": "1.0",
    "ets": 1669117705369,
    "params": {
        "resmsgid": "",
        "msgid": "2ff44354-bb9a-4dce-93ba-053f97587df6",
        "err": "",
        "status": "SUCCESSFUL",
        "errmsg": ""
    },
    "responseCode": "OK"
}
```
{% endswagger-response %}
{% endswagger %}

Sample Schema Request Payload

```json
{
  "schema": "{   \"$schema\": \"http://json-schema.org/draft-07/schema\",   \"type\": \"object\",   \"properties\": {      \"Place\": {         \"$ref\": \"#/definitions/Place\"      }   },   \"required\": [      \"Place\"   ],   \"title\": \"Place\",   \"definitions\": {      \"Place\": {         \"$id\": \"#/properties/Place\",         \"type\": \"object\",         \"title\": \"The Place Schema\",         \"required\": [            \"name\",            \"city\"],         \"properties\": {            \"name\": {               \"type\": \"string\"            },            \"city\": {               \"type\": \"string\"            }},   \"_osConfig\": {      \"privateFields\": [         \"name\"      ],      \"signedFields\": [],      \"roles\": [         \"anonymous\"      ],      \"ownershipAttributes\": [],      \"attestationPolicies\": [         {            \"name\": \"schemaAttestation\",            \"conditions\": \"(ATTESTOR#$.[*]#.contains('board-cbse'))\",            \"type\": \"MANUAL\",            \"attestorPlugin\": \"did:internal:ClaimPluginActor?entity=board-cbse\",            \"attestationProperties\": {}         }      ],      \"credentialTemplate\": {         \"@context\": [            \"https://www.w3.org/2018/credentials/v1\",            \"{someUrlForTemplate}\"         ],         \"type\": [            \"VerifiableCredential\",            \"{TypeOfCertificate}\"         ],         \"issuer\": \"{issuerName}\",         \"issuanceDate\": \"{issuanceDate}\",         \"credentialSubject\": {            \"type\": \"Place\",            \"name\": \"{name}\"},         \"evidence\": {            \"type\": \"{SomeType}\"}},      \"certificateTemplates\": {        \"html\": \"{templateUrl}\"      }   }}}}"
}
```

### Usage

#### cURL

```shell
curl --location --request PUT '{registry-url}/api/v1/Schema/{id}' \
--header 'Authorization: Bearer {access-token}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "schema": "{   \"$schema\": \"http://json-schema.org/draft-07/schema\",   \"type\": \"object\",   \"properties\": {      \"Place\": {         \"$ref\": \"#/definitions/Place\"      }   },   \"required\": [      \"Place\"   ],   \"title\": \"Place\",   \"definitions\": {      \"Place\": {         \"$id\": \"#/properties/Place\",         \"type\": \"object\",         \"title\": \"The Place Schema\",         \"required\": [            \"name\",            \"city\"],         \"properties\": {            \"name\": {               \"type\": \"string\"            },            \"city\": {               \"type\": \"string\"            }},   \"_osConfig\": {      \"privateFields\": [         \"name\"      ],      \"signedFields\": [],      \"roles\": [         \"anonymous\"      ],      \"ownershipAttributes\": [],      \"attestationPolicies\": [         {            \"name\": \"schemaAttestation\",            \"conditions\": \"(ATTESTOR#$.[*]#.contains('board-cbse'))\",            \"type\": \"MANUAL\",            \"attestorPlugin\": \"did:internal:ClaimPluginActor?entity=board-cbse\",            \"attestationProperties\": {}         }      ],      \"credentialTemplate\": {         \"@context\": [            \"https://www.w3.org/2018/credentials/v1\",            \"{someUrlForTemplate}\"         ],         \"type\": [            \"VerifiableCredential\",            \"{TypeOfCertificate}\"         ],         \"issuer\": \"{issuerName}\",         \"issuanceDate\": \"{issuanceDate}\",         \"credentialSubject\": {            \"type\": \"Place\",            \"name\": \"{name}\"},         \"evidence\": {            \"type\": \"{SomeType}\"}},      \"certificateTemplates\": {        \"html\": \"{templateUrl}\"      }   }}}}"
}'
```

#### HTTPie

```
printf '{
  "schema": "{   \"$schema\": \"http://json-schema.org/draft-07/schema\",   \"type\": \"object\",   \"properties\": {      \"Place\": {         \"$ref\": \"#/definitions/Place\"      }   },   \"required\": [      \"Place\"   ],   \"title\": \"Place\",   \"definitions\": {      \"Place\": {         \"$id\": \"#/properties/Place\",         \"type\": \"object\",         \"title\": \"The Place Schema\",         \"required\": [            \"name\",            \"city\"],         \"properties\": {            \"name\": {               \"type\": \"string\"            },            \"city\": {               \"type\": \"string\"            }},   \"_osConfig\": {      \"privateFields\": [         \"name\"      ],      \"signedFields\": [],      \"roles\": [         \"anonymous\"      ],      \"ownershipAttributes\": [],      \"attestationPolicies\": [         {            \"name\": \"schemaAttestation\",            \"conditions\": \"(ATTESTOR#$.[*]#.contains('board-cbse'))\",            \"type\": \"MANUAL\",            \"attestorPlugin\": \"did:internal:ClaimPluginActor?entity=board-cbse\",            \"attestationProperties\": {}         }      ],      \"credentialTemplate\": {         \"@context\": [            \"https://www.w3.org/2018/credentials/v1\",            \"{someUrlForTemplate}\"         ],         \"type\": [            \"VerifiableCredential\",            \"{TypeOfCertificate}\"         ],         \"issuer\": \"{issuerName}\",         \"issuanceDate\": \"{issuanceDate}\",         \"credentialSubject\": {            \"type\": \"Place\",            \"name\": \"{name}\"},         \"evidence\": {            \"type\": \"{SomeType}\"}},      \"certificateTemplates\": {        \"html\": \"{templateUrl}\"      }   }}}}"
}'| http  PUT '{registry-url}/api/v1/Schema' \
 Authorization:'Bearer {access-token}' \
 Content-Type:'application/json'
```

`{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`
