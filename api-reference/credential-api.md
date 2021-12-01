# Credential API

{% swagger method="get" path="" baseUrl="/api/v1/EntityName/Id" summary="Download W3 verifiable credential" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Accept" type="" required="true" %}
**`application/vc+ld+json`**
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Signed JSON Response body" %}
```javascript
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1"
  ],
  "type": [
    "VerifiableCredential",
    "ProofOfSkill"
  ],
  "credentialSubject": "p1",
  "issuer": "https://moh.gov/india",
  "issuanceDate": "2021-10-01T01:01:01.00Z",
  "proof": {
    "type": "Ed25519Signature2018",
    "created": "2021-11-09T16:09:55Z",
    "verificationMethod": "did:india",
    "proofPurpose": "assertionMethod",
    "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..rY3N902f378UqEo7kG9CWOlF2CVm10uzFClPn5w0QLELRPGmPIJYrvDtiZgrYGlFwG9ui1F9-Kt6KauhIDLPCw"
  }
}
```


{% endswagger-response %}
{% endswagger %}

