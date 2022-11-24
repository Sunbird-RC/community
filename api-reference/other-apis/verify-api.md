# Verify API

{% swagger method="post" path="/api/v1/verify" baseUrl=" " summary="" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="..." required="true" %}
Signed Credentials
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" required="true" %}
Set to 

`Bearer {access-token}`
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Verified Signature Response" %}
```json
"verified": true,
    "results": [
        {
            ...
        }
    ]
}
```
{% endswagger-response %}
{% endswagger %}

> Below is sample Request Body

```json
{
    "signedCredentials": {
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
            "name": "Ade Hastuti",
            "account_number": "229899429"
        },
        "issuer": "did:web:openg2p",
        "proof": {
            "type": "RsaSignature2018",
            "created": "2022-11-23T10:25:57Z",
            "verificationMethod": "did:india",
            "proofPurpose": "assertionMethod",
            "jws": "eyJhbGciOiJQUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..IiTScpcohbqKX61qLqf9RUDVKm0dXGREHScvOCITy86k7LSD_UH9izT8SRDg-nG9D4KI5VGQDhNahNb3g-UYcpnmUsXVnty_KdIdQE008PZPDeQRGL06V9HCz5txIUsDhGVhkaOoXUNoCV9asjIfOjMd9mZsEQlcGVZ_7NiUB6QYU0Dl4XjCXxMY3dv81Nc6wNlM4Tv_Y8QTqvwM-cir34LypwnSb8MFeBJA7k2gN2Undm0XUlgrg717Zz9t7YmDOWnCbt5_Ct1CW8MaviPkREZoo1vQs8W92piBlVyfX4TqMnysByoME7umyuJmIru5KRPO7PVrNorN8b8GCwYSXg"
        }
    }
}
```

### Usage



#### cURL

```shell
curl --location --request POST '{registry-url}/api/v1/verify' \
--header 'Authorization: Bearer {access-token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "signedCredentials": {
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
            "name": "Ade Hastuti",
            "account_number": "229899429"
        },
        "issuer": "did:web:openg2p",
        "proof": {
            "type": "RsaSignature2018",
            "created": "2022-11-23T10:25:57Z",
            "verificationMethod": "did:india",
            "proofPurpose": "assertionMethod",
            "jws": "eyJhbGciOiJQUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..IiTScpcohbqKX61qLqf9RUDVKm0dXGREHScvOCITy86k7LSD_UH9izT8SRDg-nG9D4KI5VGQDhNahNb3g-UYcpnmUsXVnty_KdIdQE008PZPDeQRGL06V9HCz5txIUsDhGVhkaOoXUNoCV9asjIfOjMd9mZsEQlcGVZ_7NiUB6QYU0Dl4XjCXxMY3dv81Nc6wNlM4Tv_Y8QTqvwM-cir34LypwnSb8MFeBJA7k2gN2Undm0XUlgrg717Zz9t7YmDOWnCbt5_Ct1CW8MaviPkREZoo1vQs8W92piBlVyfX4TqMnysByoME7umyuJmIru5KRPO7PVrNorN8b8GCwYSXg"
        }
    }
}'
```

#### HTTPie

```shell
printf '{
    "signedCredentials": {
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
            "name": "Ade Hastuti",
            "account_number": "229899429"
        },
        "issuer": "did:web:openg2p",
        "proof": {
            "type": "RsaSignature2018",
            "created": "2022-11-23T10:25:57Z",
            "verificationMethod": "did:india",
            "proofPurpose": "assertionMethod",
            "jws": "eyJhbGciOiJQUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..IiTScpcohbqKX61qLqf9RUDVKm0dXGREHScvOCITy86k7LSD_UH9izT8SRDg-nG9D4KI5VGQDhNahNb3g-UYcpnmUsXVnty_KdIdQE008PZPDeQRGL06V9HCz5txIUsDhGVhkaOoXUNoCV9asjIfOjMd9mZsEQlcGVZ_7NiUB6QYU0Dl4XjCXxMY3dv81Nc6wNlM4Tv_Y8QTqvwM-cir34LypwnSb8MFeBJA7k2gN2Undm0XUlgrg717Zz9t7YmDOWnCbt5_Ct1CW8MaviPkREZoo1vQs8W92piBlVyfX4TqMnysByoME7umyuJmIru5KRPO7PVrNorN8b8GCwYSXg"
        }
    }
}'| http POST '{registry-url}/api/v1/verify' \
 Authorization:'Bearer {access-token}' \
 Content-Type:'application/json'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`
