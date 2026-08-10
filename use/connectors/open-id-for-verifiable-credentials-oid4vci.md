# Open ID for Verifiable Credentials (OID4VCI)

### What it is

`oid4vc-service` is a **protocol façade**: it speaks the wallet-facing OpenID4VC protocols (OpenID4VCI 1.0 for issuance, OpenID4VP 1.0/draft-23 for presentation) on the outside, and delegates everything else to Sunbird RC's existing services on the inside:

* **credentials-service** (`/credentials/issue`, `/credentials/verify`) — builds and signs the verifiable credential, stores it
* **identity-service** (`/utils/sign`, `/utils/sign-jwt`, `/utils/sign-sd-jwt`, `/utils/sign-mdoc`, `/did/resolve`, `/.well-known/jwks.json`) — all key operations; keys stay in Vault
* **credential-schema** (`/credential-schema/oid4vci-configs`) — drives the published issuer metadata

`oid4vc-service` itself holds **no credential keys and no credential storage** — only its own short-lived session state and (optionally) its own OAuth/protocol signing key, delegated to identity-service.

### Supported credential formats

Chosen per credential type via the schema's `oid4vciConfig.oid4vciFormats`:

| Format        | Signed by (identity-service)                            | Selective disclosure | Claim shape                                        |
| ------------- | ------------------------------------------------------- | -------------------- | -------------------------------------------------- |
| `ldp_vc`      | `/utils/sign` (Ed25519 linked-data proof)               | No                   | W3C `credentialSubject`                            |
| `jwt_vc_json` | `/utils/sign-jwt` (ES256 JWS, W3C VC-JWT convention)    | No                   | W3C `credentialSubject`, nested under a `vc` claim |
| `vc+sd-jwt`   | `/utils/sign-sd-jwt` (ES256, IETF SD-JWT VC)            | Yes                  | Flat top-level claims, digests in `_sd`            |
| `mso_mdoc`    | `/utils/sign-mdoc` (ES256 COSE\_Sign1, ISO/IEC 18013-5) | Yes (per-element)    | `{namespace: {elementIdentifier: value}}`          |

Credentials can also carry a [W3C VC Render Method](https://www.w3.org/TR/vc-render-method/) entry (inline SVG or a hosted URL) for visual rendering by wallets.

### OpenID4VCI — credential issuance flow

1. **Discovery** — the wallet calls `GET /.well-known/openid-credential-issuer`; `oid4vc-service` looks up opted-in schemas live from credential-schema and returns one `credential_configurations_supported` entry per `<schemaId>_<format>` combination.
2. **Offer creation** (issuer-side) — the registry or another issuer system calls `POST /oid4vc/offer` with `{credential_configuration_id, format, claims}` and gets back `{offer_id, credential_offer_uri, credential_offer, qr_data}`.
3. **Wallet dereferences the offer** — `GET /oid4vc/offer/:id` returns the `credential_offer`, including a pre-authorized\_code grant.
4. **Token exchange** — the wallet calls `POST /oid4vc/token` with the pre-authorized code (single-use — a replay returns `400 invalid_grant`) and receives a short-lived access token plus a single-use `c_nonce`.
5. **Credential request (proof-of-possession)** — the wallet calls `POST /oid4vc/credential` with a PoP JWT proving control of its holder key (DID-bound via `kid`, or an inline `jwk`/`did:jwk`). `oid4vc-service` verifies the proof, then delegates to credentials-service (which delegates the actual signing to identity-service) and returns the signed credential.

### OpenID4VP — presentation verification flow

1. **Verifier creates a request** — `POST /vp/request` with a DCQL query returns `{transaction_id, request_uri, qr_data}`. By default the request object is a signed JAR (`client_id` is a `did:...`); an unsigned mode and a legacy pre-draft-22 shape are available for wallets on older drafts (see Configuration below).
2. **Wallet fetches and answers** — `GET /vp/request-object/:id` returns the request (JWS or plain JSON depending on mode); the wallet responds via `POST /vp/response` (`direct_post`) with its `vp_token`.
3. **Verification chain** — `oid4vc-service` checks, in order: holder/device signature, nonce/session-transcript freshness, each embedded credential's signature and revocation status, holder binding, and finally DCQL satisfaction against the disclosed claims.
4. **Verifier polls the result** — `GET /vp/status/:id` returns `{verified, checks: {holderSignature, nonce, credentialSignatures, holderBinding, revocation, dcql}, claims, holderDid}`. All six checks reporting `OK` is the full proof that the presentation is genuine, unrevoked, holder-bound, and satisfies the verifier's query.

### Key configuration

| Env var                          | Default                             | Purpose                                                                                                          |
| -------------------------------- | ----------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `PUBLIC_URL`                     | `http://localhost:3400`             | must be the externally-reachable HTTPS URL in production — embedded in minted tokens and request objects         |
| `ISSUER_DID`                     | _(auto-generated on boot if blank)_ | fallback issuer DID; should be pinned in production                                                              |
| `OID4VP_ENABLED`                 | `true`                              | set `false` to run issuance-only                                                                                 |
| `DRAFT13_COMPAT_MODE`            | `false`                             | emit OpenID4VCI draft-13 shapes for older wallet implementations                                                 |
| `OID4VP_SIGN_REQUEST`            | `true`                              | sign OID4VP request objects as a JAR; requires a wallet-resolvable `VERIFIER_DID` (a `did:web`, not a `did:rcw`) |
| `OID4VP_LEGACY_CLIENT_ID_SCHEME` | `false`                             | emit the pre-draft-22 unsigned request shape for wallets that don't parse the prefixed `client_id` convention    |
| `SESSION_STORE`                  | `memory`                            | set to `redis` for any multi-replica deployment                                                                  |
| `ENABLE_AUTH`                    | `false`                             | require a Keycloak bearer token on `POST /oid4vc/offer` (the only internal, non-wallet-facing route)             |

See `services/oid4vc-service/README.md` §4 in the sunbird-rc-core repository for the complete reference, including TTL settings, the Keycloak auth setup, and per-schema issuer DID behavior.

### Getting started

```bash
docker compose up -d db vault redis
bash enable-v2.sh                                 # inits vault, brings up identity/credential-schema/credential
docker compose --profile oid4vc up -d --build oid4vc-service
curl -s http://localhost:3400/health
```

For the full local deployment walkthrough (including Apple Silicon build notes), manual testing guide, and production hardening checklist, see `services/oid4vc-service/README.md` §5–§8 in the repository.

### Verified wallet interop

Beyond the self-driven test harness (all four formats issue and verify end-to-end with a `verified: true` result), `jwt_vc_json` and `vc+sd-jwt` issuance and presentation have been confirmed against independent third-party wallet implementations during interop testing.
