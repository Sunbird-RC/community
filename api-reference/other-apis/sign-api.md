# Sign API

{% swagger method="post" path="/utils/sign" baseUrl=" " summary="Sign the Payload without storing" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="authorization" required="true" %}
Set to 

`Bearer {access-token}`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="..." type="Object" required="true" %}
Json Object with data key and its value representing what needs to be signed and its credentialTemplate as another key
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully generated signed credentials" %}
```javascript
{
    "id": "sunbird-rc.utils.sign",
    "ver": "1.0",
    "ets": 1669196000115,
    "params": {
        "resmsgid": "",
        "msgid": "f83bd8d6-cef5-43ff-be74-558e5f5121cc",
        "err": "",
        "status": "SUCCESSFUL",
        "errmsg": ""
    },
    "responseCode": "OK",
    "result": {
        "@context": [
            "https://www.w3.org/2018/credentials/v1",
            "{context_url}"
        ],
        "type": [
            "VerifiableCredential"
        ],
        "issuanceDate": "2021-08-27T10:57:57.237Z",
        "credentialSubject": {
            "type": "Beneficiary",
            "name": "Ade Hastuti"
        },
        "issuer": "did:web:openg2p",
        "proof": {
            "type": "RsaSignature2018",
            "created": "2022-11-23T09:33:24Z",
            "verificationMethod": "did:india",
            "proofPurpose": "assertionMethod",
            "jws": "eyJhbGciOiJQUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..WG0GIEQS6etth5CHzQfk73rtsGKuiQE3t_zYNKUuhPhTSECa34YWEOTLDDO2iUpufNO8DcLXLSme0XB2HVyS7E0j5sHOWHPDyJiCZRlIFO2cQl1dLCXO84p7KVuEat2AmgDBQjlMUFUQk3PhBhzRhQc0DP6EhGHcXlZMgizcwlN9japYcKuHGCXGj6VXkwoNKFcSAfvkUIaWLlPuy2pvYiNYqek-07PIVtBBQN0rWcGwe3MeHFvFkURwUK5adrnv9TKWv50AnopZSS750glIxFAXv5tfg71c_XQCwaTRa6FUGO-vCQgO8meN3yZ6EqS8oeu1jiRc5ohNGzbyOjQa3A"
        }
    }
}
```
{% endswagger-response %}
{% endswagger %}

> Sample Request

```json
{
    "id": "sunbird-rc.utils.sign",
    "request": {
        "data": {
            "name": "Ade Hastuti",
            "date_of_birth": "2022-08-01",
            "gender": "Female"
        },
        "credentialTemplate": {
            "@context": [
                "https://www.w3.org/2018/credentials/v1",
                "{context_url}"
            ],
            "type": [
                "VerifiableCredential"
            ],
            "issuanceDate": "2021-08-27T10:57:57.237Z",
            "credentialSubject": {
                "type": "Beneficiary",
                "name": "{{name}}",
            },
            "issuer": "did:web:openg2p"
        }
    }
}
```

### Usage

#### cURL

```shell
curl --location --request POST '{registry-url}/utils/sign' \
--header 'Authorization: Bearer {access-token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "id": "sunbird-rc.utils.sign",
    "request": {
        "data": {
            "name": "Ade Hastuti",
            "date_of_birth": "2022-08-01",
            "gender": "Female"
        },
        "credentialTemplate": {
            "@context": [
                "https://www.w3.org/2018/credentials/v1",
                "{context_url}"
            ],
            "type": [
                "VerifiableCredential"
            ],
            "issuanceDate": "2021-08-27T10:57:57.237Z",
            "credentialSubject": {
                "type": "Beneficiary",
                "name": "{{name}}",
            },
            "issuer": "did:web:openg2p"
        }
    }
}'
```

#### HTTPie

```shell
printf '{
    "id": "sunbird-rc.utils.sign",
    "request": {
        "data": {
            "name": "Ade Hastuti",
            "date_of_birth": "2022-08-01",
            "gender": "Female"
        },
        "credentialTemplate": {
            "@context": [
                "https://www.w3.org/2018/credentials/v1",
                "{context_url}"
            ],
            "type": [
                "VerifiableCredential"
            ],
            "issuanceDate": "2021-08-27T10:57:57.237Z",
            "credentialSubject": {
                "type": "Beneficiary",
                "name": "{{name}}",
            },
            "issuer": "did:web:openg2p"
        }
    }
}'| http POST '{registry-url}/utils/sign' \
 Authorization:'Bearer {access-token}' \
 Content-Type:'application/json'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`
