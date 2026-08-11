# OID4VC Service API

The **oid4vc-service** implements OpenID for Verifiable Credential Issuance (**OID4VCI**) and OpenID for Verifiable Presentations (**OID4VP**) for the Sunbird RC stack. It issues and verifies credentials in multiple formats — `jwt_vc_json`, `ldp_vc`, `vc+sd-jwt`, and `mso_mdoc` — and supports the [W3C VC Render Method](https://www.w3.org/TR/vc-render-method/) for rendering credentials visually.

> **Prerequisite:** the full Sunbird RC stack must be running (db, vault, redis, `identity-service`, `credential-schema`, `credential-service`, `oid4vc-service`)

#### Base URLs

| Variable          | Default value           | Service                   |
| ----------------- | ----------------------- | ------------------------- |
| `identity_base`   | `http://localhost:3332` | identity-service          |
| `schema_base`     | `http://localhost:3333` | credential-schema service |
| `credential_base` | `http://localhost:3000` | credentials-service       |
| `oid4vc_base`     | `http://localhost:3400` | oid4vc-service            |

***

### 0. Discovery

Read-only endpoints for service health and OID4VC/OIDC metadata discovery.

#### Health check

```
GET {{oid4vc_base}}/health
```

Returns `200 OK` when the service is up.

#### Issuer metadata

```
GET {{oid4vc_base}}/.well-known/openid-credential-issuer
```

Returns the OID4VCI Credential Issuer Metadata document (supported credential configurations, formats, endpoints).

#### Authorization Server metadata

```
GET {{oid4vc_base}}/.well-known/openid-configuration
```

Returns the OAuth2/OIDC Authorization Server metadata document.

#### JWKS

```
GET {{oid4vc_base}}/.well-known/jwks.json
```

Returns the issuer's public JSON Web Key Set, used by wallets/verifiers to validate signatures.

***

### 1. Setup — Schemas & DIDs

Endpoints on `identity-service` and `credential-schema` used to prepare issuer/holder DIDs and an OID4VCI-enabled credential schema before running any issuance flow.

#### Generate a DID

```
POST {{identity_base}}/did/generate
Content-Type: application/json
```

**Body**

```json
{
  "content": [
    {
      "alsoKnownAs": ["postman-issuer"],
      "services": [],
      "method": "rcw"
    }
  ]
}
```

> Pass `"keyPairType": "JsonWebKey2020"` to generate an EC P-256 key pair (required for `mso_mdoc` issuer DIDs — see Section 6).

**Response** — array of DID Documents, e.g. `id: "did:rcw:..."`. Used to generate issuer, holder, and impostor DIDs.

#### Create an OID4VCI-enabled credential schema

```
POST {{schema_base}}/credential-schema
Content-Type: application/json
```

**Body**

```json
{
  "schema": {
    "type": "https://w3c-ccg.github.io/vc-json-schemas/",
    "version": "1.0.0",
    "id": "{{$guid}}",
    "name": "OID4VC Pilot Credential",
    "author": "{{issuer_did}}",
    "authored": "2026-01-01T00:00:00.000Z",
    "schema": {
      "$id": "OID4VC-Pilot-Credential-1.0",
      "$schema": "https://json-schema.org/draft/2019-09/schema",
      "description": "Pilot credential for OID4VC Postman collection testing",
      "type": "object",
      "properties": { "name": { "type": "string" } },
      "required": ["name"],
      "additionalProperties": true
    }
  },
  "tags": ["oid4vc-pilot"],
  "status": "PUBLISHED",
  "oid4vciConfig": {
    "oid4vciEnabled": true,
    "oid4vciFormats": ["ldp_vc", "jwt_vc_json"],
    "display": [{ "name": "OID4VC Pilot Credential", "locale": "en-US" }]
  }
}
```

The key part enabling OID4VCI is the `oid4vciConfig` block:

<table><thead><tr><th width="169">Field</th><th>Description</th></tr></thead><tbody><tr><td><code>oid4vciEnabled</code></td><td>Must be <code>true</code> for the schema to be offerable via <code>/oid4vc/offer</code>.</td></tr><tr><td><code>oid4vciFormats</code></td><td>One or more of <code>jwt_vc_json</code>, <code>ldp_vc</code>, <code>vc+sd-jwt</code>, <code>mso_mdoc</code>.</td></tr><tr><td><code>display</code></td><td>Human-readable display metadata surfaced in issuer metadata / wallets.</td></tr><tr><td><code>vct</code></td><td>(SD-JWT only) the Verifiable Credential Type identifier.</td></tr><tr><td><code>mdoc</code></td><td>(mso_mdoc only) <code>{ docType, namespace }</code> per ISO/IEC 18013-5.</td></tr><tr><td><code>renderMethod</code></td><td>(optional) embeds a W3C VC Render Method template.</td></tr></tbody></table>

**Response** — `201 Created` with the persisted schema, including `schema.id`.

#### List OID4VCI-enabled schemas

```
GET {{schema_base}}/credential-schema/oid4vci-configs
```

Returns an array of schemas that have `oid4vciEnabled: true`, each including `schemaId`, `formats`, and (where applicable) `mdoc.docType` / `mdoc.namespace`. Used by wallets/issuers to discover what can be offered.

***

### 2. OID4VCI — Credential Issuance Flow

The core issuance flow (pre-authorized code grant), demonstrated here for `jwt_vc_json` but identical in shape for `ldp_vc`, `vc+sd-jwt`, and `mso_mdoc` — only `format` and the request/response payload shape differ.

#### Create a credential offer

```
POST {{oid4vc_base}}/oid4vc/offer
Content-Type: application/json
```

**Body**

```json
{
  "credential_configuration_id": "{{schema_id}}",
  "format": "jwt_vc_json",
  "claims": { "name": "Postman Test Holder" }
}
```

**Response** — `201 Created`

```json
{
  "offer_id": "...",
  "credential_offer": {
    "credential_configuration_ids": ["{{schema_id}}_jwt_vc_json"],
    "grants": {
      "urn:ietf:params:oauth:grant-type:pre-authorized_code": {
        "pre-authorized_code": "..."
      }
    }
  }
}
```

> `credential_configuration_ids` is derived from the schema's `id`, suffixed with the format (e.g. `<schemaId>_jwt_vc_json`) — not from the display name, since names aren't guaranteed unique.

#### Dereference an offer (wallet-side)

```
GET {{oid4vc_base}}/oid4vc/offer/{offer_id}
```

Returns the full `credential_offer` payload (grants, `pre-authorized_code`) for a wallet that received only the `offer_id`/ offer URI.

* `404 Not Found` — offer unknown or expired.

#### Exchange the pre-authorized code for a token

```
POST {{oid4vc_base}}/oid4vc/token
Content-Type: application/x-www-form-urlencoded
```

**Body (form-urlencoded)**

| Key                   | Value                                                  |
| --------------------- | ------------------------------------------------------ |
| `grant_type`          | `urn:ietf:params:oauth:grant-type:pre-authorized_code` |
| `pre-authorized_code` | `{{pre_auth_code}}`                                    |

**Response** — `200 OK`

```json
{
  "access_token": "...",
  "c_nonce": "..."
}
```

* `400 Bad Request` — code already used or invalid (`"bad or used code"`).

#### Sign a proof-of-possession JWT (wallet stand-in)

```
POST {{identity_base}}/utils/sign-jwt
Content-Type: application/json
```

**Body**

```json
{
  "DID": "{{holder_did}}",
  "payload": { "aud": "{{oid4vc_base}}", "nonce": "{{c_nonce}}" },
  "header": { "typ": "openid4vci-proof+jwt" }
}
```

This is a test/dev convenience — in production the wallet signs this proof itself with the holder's private key. Returns `{ "jwt": "..." }`.

#### Request the credential

```
POST {{oid4vc_base}}/oid4vc/credential
Authorization: Bearer {{access_token}}
Content-Type: application/json
```

**Body**

```json
{
  "proof": { "proof_type": "jwt", "jwt": "{{pop_jwt}}" }
}
```

**Response** — `200 OK`

```json
{
  "format": "jwt_vc_json",
  "credential": "..."
}
```

* `401 Unauthorized` — missing/invalid bearer token.

#### Verify an issued credential

```
POST {{credential_base}}/credentials/verify
Content-Type: application/json
```

**Body**

```json
{
  "verifiableCredential": "{{issued_credential}}"
}
```

**Response** — `200 OK`, with per-check results, e.g.:

```json
{
  "checks": [{ "proof": "OK", "expired": "OK" }]
}
```

***

### 3. OID4VCI — Negative Tests

Documents expected error responses for common failure conditions.

| Scenario                                  | Request                                                                                                                                             | Expected result              |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
| **Replay pre-auth code**                  | `POST /oid4vc/token` with an already-used `pre-authorized_code`                                                                                     | `400` — `"bad or used code"` |
| **Unknown `credential_configuration_id`** | `POST /oid4vc/offer` with a nonexistent configuration id                                                                                            | `404` — not enabled          |
| **Unsupported format**                    | `POST /oid4vc/offer` with a `format` not enabled on the schema (e.g. requesting `vc+sd-jwt` on a schema only configured for `jwt_vc_json`/`ldp_vc`) | `400` — format not supported |
| **Dereference unknown offer**             | `GET /oid4vc/offer/00000000-0000-0000-0000-000000000000`                                                                                            | `404` — not found or expired |
| **Missing Authorization header**          | `POST /oid4vc/credential` without a `Bearer` token                                                                                                  | `401` — missing bearer token |

Example — unsupported format:

```
POST {{oid4vc_base}}/oid4vc/offer
Content-Type: application/json

{ "credential_configuration_id": "{{schema_id}}", "format": "vc+sd-jwt", "claims": {} }
```

→ `400 Bad Request`

***

### 4. OID4VP — Presentation Flow

Verifier-initiated presentation flow using [DCQL](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) (Digital Credentials Query Language) and the `direct_post` response mode.

#### Verifier creates a presentation request

```
POST {{oid4vc_base}}/vp/request
Content-Type: application/json
```

**Body**

```json
{
  "dcql_query": {
    "credentials": [
      {
        "id": "pilot_cred",
        "format": "jwt_vc_json",
        "claims": [{ "path": ["credentialSubject", "name"] }]
      }
    ]
  }
}
```

**Response**

```json
{
  "transaction_id": "...",
  "request_uri": "..."
}
```

#### Wallet fetches the request object

```
GET {{vp_request_uri}}
```

Returns a plain JSON payload — **not** a signed JWT/JAR request object, for compatibility with wallets targeting older OID4VP drafts. Includes:

```json
{
  "nonce": "...",
  "state": "...",
  "dcql_query": { ... }
}
```

#### Sign the VP token (wallet stand-in)

```
POST {{identity_base}}/utils/sign-jwt
Content-Type: application/json
```

**Body**

```json
{
  "DID": "{{holder_did}}",
  "payload": {
    "aud": "{{oid4vc_base}}",
    "nonce": "{{vp_nonce}}",
    "vp": { "verifiableCredential": ["{{issued_credential}}"] }
  },
  "header": { "typ": "JWT" }
}
```

#### Submit the Verifiable Presentation (`direct_post`)

```
POST {{oid4vc_base}}/vp/response
Content-Type: application/json
```

**Body**

```json
{
  "state": "{{vp_state}}",
  "vp_token": "{{vp_jwt}}"
}
```

**Response** — `200 OK`, `{ "status": "ok" }`.

#### Verifier polls the transaction status

```
GET {{oid4vc_base}}/vp/status/{transaction_id}
```

**Response** — `200 OK`

```json
{
  "verified": true,
  "checks": {
    "holderSignature": "OK",
    "nonce": "OK",
    "credentialSignatures": "OK",
    "holderBinding": "OK",
    "revocation": "OK",
    "dcql": "OK"
  }
}
```

***

### 5. OID4VP — Negative Tests

| Scenario                          | Request                                                                    | Expected result                                   |
| --------------------------------- | -------------------------------------------------------------------------- | ------------------------------------------------- |
| **Unknown `state`**               | `POST /vp/response` with a bogus `state`                                   | `400` — unknown or expired state                  |
| **Unknown transaction id**        | `GET /vp/status/00000000-0000-0000-0000-000000000000`                      | `404` — transaction not found                     |
| **Replay an already-verified VP** | `POST /vp/response` again with the same `state`/`vp_token`                 | `400` — transaction not pending (replay rejected) |
| **Holder-binding mismatch**       | An **impostor** DID signs someone else's credential and submits it as a VP | `403` — `"holder binding failed"`                 |

The holder-binding negative test repeats the full flow from Section 4 (fresh `/vp/request`, fetch request object) but signs the VP token with an `impostor_did` instead of `holder_did`, over the original holder's issued credential — the service must detect the DID mismatch and reject with `403`.

***

### 6. mso\_mdoc — Issuance & Verification

ISO/IEC 18013-5 mobile-document (mDL) format issuance and verification, using an EC P-256 issuer key.

> mdoc **presentation** (OID4VP) requires a wallet to build a CBOR `DeviceResponse` signed with COSE, which can't be produced in a API endpoint.

#### Generate an mdoc issuer DID (EC P-256 / JsonWebKey2020)

```
POST {{identity_base}}/did/generate
Content-Type: application/json
```

**Body**

```json
{
  "content": [
    {
      "alsoKnownAs": ["postman-mdoc-issuer"],
      "services": [],
      "method": "rcw",
      "keyPairType": "JsonWebKey2020"
    }
  ]
}
```

Response DID's `verificationMethod[0].type` is `JsonWebKey2020` with `publicKeyJwk.crv: "P-256"`.

#### Create an `mso_mdoc` schema

```
POST {{schema_base}}/credential-schema
```

Same shape as Section 1, with:

```json
"oid4vciConfig": {
  "oid4vciEnabled": true,
  "oid4vciFormats": ["mso_mdoc"],
  "display": [{ "name": "Postman mDL Credential", "locale": "en-US" }],
  "mdoc": {
    "docType": "org.iso.18013.5.1.mDL",
    "namespace": "org.iso.18013.5.1"
  }
}
```

`GET {{schema_base}}/credential-schema/oid4vci-configs` will list this schema with `formats: ["mso_mdoc"]` and the `mdoc.docType`/`mdoc.namespace` echoed back.

#### Create an mdoc offer, exchange for token, request credential

Identical to the core issuance flow (`/oid4vc/offer` → `/oid4vc/token` → sign PoP JWT → `/oid4vc/credential`), except:

* `format` is `"mso_mdoc"`
* `credential_configuration_ids[0]` equals the schema id directly (no format suffix)
* The returned `credential` is a base64url-encoded CBOR structure — it contains **no** `.` (JWT-style) or `~` (SD-JWT-style) separators.

#### Verify an issued mdoc credential

```
POST {{credential_base}}/credentials/verify
Content-Type: application/json
```

**Body**

```json
{ "verifiableCredential": "{{mdoc_credential}}" }
```

**Response**

```json
{
  "checks": [{ "proof": "OK" }],
  "docType": "org.iso.18013.5.1.mDL"
}
```

#### Negative test — tampered mdoc

Submitting a corrupted credential (e.g. appending garbage bytes) to `/credentials/verify` must fail cleanly: either `checks[0].proof: "NOK"` or a populated `errors` array — never a silent pass.

***

### 7. W3C VC Render Method

Demonstrates the [W3C VC Render Method](https://www.w3.org/TR/vc-render-method/) spec: embedding a visual (SVG) rendering template in an issued credential, addressed by content digest.

#### Create a schema with an inline SVG `renderMethod`

```
POST {{schema_base}}/credential-schema
```

```json
"oid4vciConfig": {
  "oid4vciEnabled": true,
  "oid4vciFormats": ["jwt_vc_json"],
  "display": [{ "name": "Postman Render Method Credential", "locale": "en-US" }],
  "renderMethod": {
    "type": "SvgRenderingTemplate",
    "name": "Postman Card",
    "svg": "<svg xmlns='http://www.w3.org/2000/svg' width='300' height='150'><text x='10' y='50'>{{credentialSubject.name}}</text></svg>"
  }
}
```

#### Issue the credential

Standard offer → token → PoP → `/oid4vc/credential` flow (Section 2). The decoded JWT payload's `vc.renderMethod`(or top-level `renderMethod`) contains:

```json
[
  {
    "type": "SvgRenderingTemplate",
    "id": ".../render-templates/...",
    "digestMultibase": "z..."
  }
]
```

* `id` is a URL hosting the rendered SVG template.
* `digestMultibase` is a multibase/multihash (SHA-256, base58btc) digest of the served template bytes.

#### Fetch the hosted render template & verify its digest

```
GET {render_template_url}
```

**Response** — `200 OK`, `Content-Type: image/svg+xml`, body containing `<svg`.

The consumer recomputes the multibase/multihash SHA-256 digest of the returned SVG bytes and asserts it equals the credential's `digestMultibase` — this is how a verifier confirms the rendering template hasn't been tampered with.

***

### 8. vc+sd-jwt — Issuance & Verification

IETF **SD-JWT VC** format ([draft-ietf-oauth-sd-jwt-vc](https://datatracker.ietf.org/doc/draft-ietf-oauth-sd-jwt-vc/)) with real selective disclosure.

#### Create a `vc+sd-jwt` schema

```
POST {{schema_base}}/credential-schema
```

```json
"oid4vciConfig": {
  "oid4vciEnabled": true,
  "oid4vciFormats": ["vc+sd-jwt"],
  "vct": "Postman SD-JWT Credential",
  "display": [{ "name": "Postman SD-JWT Credential", "locale": "en-US" }]
}
```

Note the additional `vct` field (Verifiable Credential Type), required for SD-JWT VC.

#### Create offer, exchange token, request credential

Same flow as Section 2 with `format: "vc+sd-jwt"`. The offer body may include selectively-disclosable claims, e.g.:

```json
{
  "credential_configuration_id": "{{sdjwt_schema_id}}",
  "format": "vc+sd-jwt",
  "claims": { "name": "SD-JWT Holder", "age_over_18": true }
}
```

**Response** from `POST /oid4vc/credential`:

```json
{
  "format": "vc+sd-jwt",
  "credential": "<jwt>~<disclosure1>~<disclosure2>~..."
}
```

The `~`-separated suffix segments are the selective disclosures per the SD-JWT format.

#### Verify the issued SD-JWT credential

```
POST {{credential_base}}/credentials/verify
Content-Type: application/json
```

```json
{ "verifiableCredential": "{{sdjwt_credential}}" }
```

**Response** — `{ "checks": [{ "proof": "OK" }] }`.

***

### Appendix: Collection Variables

The Postman collection uses these variables to pass state between requests. When implementing your own client, model equivalent state:

| Variable                                                                        | Set by                                               | Used by                                                          |
| ------------------------------------------------------------------------------- | ---------------------------------------------------- | ---------------------------------------------------------------- |
| `issuer_did`, `holder_did`, `impostor_did`                                      | `POST /did/generate`                                 | Schema `author`, PoP/VP JWT signing                              |
| `schema_id`, `mdoc_schema_id`, `render_schema_id`, `sdjwt_schema_id`            | `POST /credential-schema`                            | `credential_configuration_id` in `/oid4vc/offer`                 |
| `offer_id`                                                                      | `POST /oid4vc/offer`                                 | `GET /oid4vc/offer/{offer_id}`                                   |
| `pre_auth_code`                                                                 | `POST /oid4vc/offer` (via `credential_offer.grants`) | `POST /oid4vc/token`                                             |
| `access_token`, `c_nonce`                                                       | `POST /oid4vc/token`                                 | `POST /oid4vc/credential`(Authorization header, PoP JWT `nonce`) |
| `pop_jwt`                                                                       | `POST /utils/sign-jwt`                               | `POST /oid4vc/credential`(`proof.jwt`)                           |
| `issued_credential`, `mdoc_credential`, `render_credential`, `sdjwt_credential` | `POST /oid4vc/credential`                            | `POST /credentials/verify`, VP token payload                     |
| `vp_transaction_id`, `vp_request_uri`                                           | `POST /vp/request`                                   | `GET /vp/status/{id}`, wallet fetch of request object            |
| `vp_nonce`, `vp_state`                                                          | `GET {vp_request_uri}`                               | VP JWT payload, `POST /vp/response`                              |
| `vp_jwt`                                                                        | `POST /utils/sign-jwt`                               | `POST /vp/response` (`vp_token`)                                 |
| `render_template_url`, `render_digest`                                          | Decoded from issued render-method credential         | `GET {render_template_url}` digest check                         |
