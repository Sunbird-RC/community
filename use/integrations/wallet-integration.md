# Wallet Integration

**Applies to:** Sunbird RC `v2.1.0` and later ·&#x20;

**Service:** `oid4vc-service` ·&#x20;

**Protocols:** OpenID4VCI 1.0, OpenID4VP 1.0 / draft-23

From v2.1.0, Sunbird RC speaks the wallet protocols directly. A conformant wallet can receive a credential from a Sunbird RC issuer and present it back to a Sunbird RC verifier with **no Sunbird-specific integration on the wallet side**— no SDK to embed, no issuer pre-registration, no proprietary API.

This page covers the two demo apps, the issuer and verifier walkthroughs, the wallets we have verified against, and the configuration knobs that exist purely for wallet compatibility.

***

### 1. Demo Apps

Two reference apps demonstrate the two halves of the flow. Both are thin UIs over the public `oid4vc-service` endpoints — no privileged access, nothing a third party couldn't build.

<table><thead><tr><th width="102">Demo</th><th width="441">What it does</th><th>Link</th></tr></thead><tbody><tr><td><strong>Issuer app</strong></td><td>Creates a credential offer for a chosen schema and format, renders the <code>openid-credential-offer://</code> QR code, and shows the offer's progress through token exchange and credential issuance.</td><td>🔗 <a href="https://98.70.36.106.sslip.io/issuer-portal/"><strong>ISSUER APP</strong></a></td></tr><tr><td><strong>Verifier app</strong></td><td>Builds a DCQL query, renders the <code>openid4vp://</code> QR code, and polls <code>GET /vp/status/:id</code>to display the six-point verification result live.</td><td>🔗 <a href="https://98.70.36.106.sslip.io/verifier-app/"><strong>VERIFIER APP</strong></a></td></tr></tbody></table>

***

### 2. End-to-End Steps

Two walkthroughs, both driven entirely from the demo apps. §2.1 gets a credential **into** a wallet; §2.2 gets one **out** and verified.

**You need:** the stack running with `oid4vc-service` deployed, and a supported wallet (§3) installed on a phone or open in a browser.

> Because a phone can't reach `localhost`, `oid4vc-service` must be exposed on a URL the phone resolves, with `OID4VC_PUBLIC_URL` set to it — that value goes into the QR deep links and the proof-of-possession `aud`claim, so a mismatch fails PoP rather than merely failing to connect.

#### 2.1 Issuer flow — credential into the wallet

**Issuer app** → user login (`issuer.staff` / `Passw0rd!`) → pick an existing issuer → **Add New Registry** entry, or choose from existing registry data → fill in the required template data → **accept**.

Scan the credential-offer QR with the wallet. **The credential lands in the wallet.**

#### 2.2 Verifier flow — credential out of the wallet

**Verifier app** → pick or write a DCQL query → scan the presentation-request QR with the same wallet → consent to the disclosure.

The verifier app's status panel then shows the result — all six checks `OK` means verified:

`holderSignature` · `nonce` · `credentialSignatures` · `holderBinding` · `revocation` · `dcql`

Together they prove that the presenter controls the holder key, that the credential is genuine, unrevoked, and bound to that same holder, and that the disclosed claims satisfy exactly what the query asked for. Any one failing rejects the presentation, and the panel shows which stage failed.

#### 2.3 Before the demo works: schema opt-in

One setup step is easy to miss — **a schema is invisible to wallets until it carries an `oid4vciConfig` block**(`oid4vciEnabled: true`, plus the `oid4vciFormats` list that decides which formats it can be issued in). Discovery is a live lookup rather than a cache, so a schema opted in now shows up immediately.

For the curl-level setup and API walkthrough, see `services/oid4vc-service/README.md` — §5 for deployment, §6 for the full manual testing guide.

#### 2.4 If something fails

| Symptom                                                        | Meaning                                                                                                     |
| -------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| Credential doesn't appear in the wallet after accepting        | Check the wallet reached `OID4VC_PUBLIC_URL`, not `localhost` — an `aud`mismatch fails proof-of-possession. |
| Wallet won't parse the offer QR at all                         | Wallet likely expects OpenID4VCI draft-13. Try `DRAFT13_COMPAT_MODE=true` (§4).                             |
| Wallet rejects the presentation request before showing consent | `client_id` convention mismatch. Try `OID4VP_LEGACY_CLIENT_ID_SCHEME=true` (§4).                            |
| Credential type not offered in the issuer app                  | Schema has no `oid4vciConfig`, or the format isn't in `oid4vciFormats`(§2.3).                               |
| `holderBinding` fails                                          | Wallet presented with a different key than it proved possession of at issuance.                             |
| `dcql` fails but every other check is `OK`                     | Crypto is fine; the disclosure doesn't answer the query.                                                    |

***

### 3. Verified Wallets

Two independently-implemented third-party wallets have been tested against a deployed instance, in addition to the self-driven harness that covers all four formats.

#### Paradym Wallet

* **Website:** https://paradym.id
* **Source:** https://github.com/animo/paradym-wallet
* **Built by:** [Animo Solutions](https://animo.id/) — open source, built on [Credo](https://github.com/openwallet-foundation/credo-ts) (OpenWallet Foundation), tracking current OpenID4VP drafts.

**Tested role:** OID4VP **Holder** only — Sunbird RC acting as the **Verifier**, i.e. the §2.4 flow. No OID4VCI / issuance-role testing was done against Paradym.

| Role tested                  | Formats                    | Result                               |
| ---------------------------- | -------------------------- | ------------------------------------ |
| Holder — OID4VP presentation | `jwt_vc_json`, `vc+sd-jwt` |  `verified: true`, all 6 checks `OK` |

**Compatibility flags required:** none. Paradym parses the default draft-23 shape, so it works against a stock configuration.

> The default signed mode needs a `VERIFIER_DID` the _wallet_ can resolve, which in practice means a `did:web`. The out-of-box Compose stack provisions a `did:rcw` — resolvable only by `identity-service` — and therefore ships with `OID4VP_SIGN_REQUEST=false`. To exercise the signed path, set `OID4VC_VERIFIER_DID` to a `did:web` and `OID4VP_SIGN_REQUEST=true`.

#### walt.id Wallet

* **Website:** https://walt.id/wallet · hosted demo at `wallet.demo.walt.id`

Tested in both roles, with no local deployment or intermediary backend — its offer-acceptance API takes the same `openid-credential-offer://` deep link from §2.3 directly, with no issuer pre-registration.

| Role tested                  | Formats                    | Result                               |
| ---------------------------- | -------------------------- | ------------------------------------ |
| Holder — OID4VCI issuance    | `jwt_vc_json`, `vc+sd-jwt` | credential received into wallet      |
| Holder — OID4VP presentation | `vc+sd-jwt`                |  `verified: true`, all 6 checks `OK` |

**Compatibility flags required:** `OID4VP_LEGACY_CLIENT_ID_SCHEME=true`. walt.id targets an older OID4VP draft where `client_id_scheme` is a separate field; in interop testing it rejected both the signed `did:` `client_id` and the prefixed `redirect_uri:` form.

#### Summary

| Wallet             | OID4VCI (issuance)         | OID4VP (presentation)      | Flags needed                          |
| ------------------ | -------------------------- | -------------------------- | ------------------------------------- |
| **Paradym Wallet** | not tested                 | `jwt_vc_json`, `vc+sd-jwt` | none (defaults)                       |
| **walt.id Wallet** | `jwt_vc_json`, `vc+sd-jwt` |  `vc+sd-jwt`               | `OID4VP_LEGACY_CLIENT_ID_SCHEME=true` |

`ldp_vc` and `mso_mdoc` are verified end-to-end in the self-driven harness but have not yet been exercised against a third-party wallet.

***

### 4. Wallet Compatibility Configuration

Three flags exist solely to accommodate wallets at different points on the standards timeline. All default to the current spec shape; none affect the credential itself, only the protocol envelope.

<table><thead><tr><th>Flag</th><th width="150">Default</th><th>Effect</th></tr></thead><tbody><tr><td><code>DRAFT13_COMPAT_MODE</code></td><td><code>false</code></td><td>Emit OpenID4VCI <strong>draft-13</strong> shapes instead of final 1.0: <code>credentials_supported</code> in metadata, <code>credentials</code> in the offer object, <code>user_pin_required</code> in the grant, and <code>c_nonce</code> retained in the token response. Needed by MOSIP Inji Wallet today. Isolated to <code>oid4vci.service.ts</code>.</td></tr><tr><td><code>OID4VP_SIGN_REQUEST</code></td><td><code>true</code> (service default)</td><td>Sign the OID4VP request object as a JAR with a <code>did:</code>-prefixed <code>client_id</code>. Set <code>false</code> for unsigned plain JSON; also overridable per request via <code>{"signed": false}</code>.</td></tr><tr><td><code>OID4VP_LEGACY_CLIENT_ID_SCHEME</code></td><td><code>false</code></td><td>Emit the pre-draft-22 shape: <code>client_id</code> = the bare <code>response_uri</code>, <code>client_id_scheme: "redirect_uri"</code> as a separate field, unsigned. Needed by walt.id.</td></tr></tbody></table>

The three OID4VP request modes:

| Mode              | `client_id`                                    | Signing                                | Selected by                           |
| ----------------- | ---------------------------------------------- | -------------------------------------- | ------------------------------------- |
| `signed`(default) | `did:<VERIFIER_DID>`                           | JWS, `application/oauth-authz-req+jwt` | `OID4VP_SIGN_REQUEST` unset / `true`  |
| `unsigned`        | `redirect_uri:<response_uri>`                  | none, plain JSON                       | `{"signed": false}` per request       |
| `legacy`          | `<response_uri>` + separate `client_id_scheme` | none, plain JSON                       | `OID4VP_LEGACY_CLIENT_ID_SCHEME=true` |

Per spec the `redirect_uri` client\_id scheme MUST NOT be signed, which is why choosing signed mode forces the `did:`prefix.

***

### 5. What the Wallet Actually Talks To

Wallets only ever touch these. `POST /oid4vc/offer` and `POST /vp/request` are issuer/verifier-side calls.

**Issuance (OpenID4VCI):**

```
GET  /.well-known/openid-credential-issuer   discovery — live from credential-schema's opted-in schemas
GET  /oid4vc/offer/:id                       dereference the offer deep link
POST /oid4vc/token                           pre-authorized_code grant → access_token + c_nonce
POST /oid4vc/credential                      proof-of-possession JWT → signed credential
```

**Presentation (OpenID4VP):**

```
GET  /vp/request-object/:id                  fetch the request object (JWS or plain JSON)
POST /vp/response                            direct_post — submit the vp_token
```

**Deep links produced:** `openid-credential-offer://...` for offers, `openid4vp://...` for presentation requests. Both come back as `qr_data`.

Holder binding accepts either a resolvable DID (`kid` in the PoP JWT) or an inline JWK / `did:jwk`, so wallets that don't publish DIDs are supported.

`oid4vc-service` holds **no credential keys and no credential storage** — only short-lived session state and its own protocol signing key, which itself lives in Vault via `identity-service`.

***

### 6. Adding a New Wallet

When a wallet fails to interop, the fault is almost always in the protocol envelope rather than the credential:

1. **Offer isn't parsed** → the wallet likely expects OpenID4VCI draft-13. Try `DRAFT13_COMPAT_MODE=true`.
2. **Presentation request rejected before consent** → `client_id` convention mismatch. Try `OID4VP_LEGACY_CLIENT_ID_SCHEME=true`, or `{"signed": false}` per request.
3. **`406` on the request object** → the wallet's `Accept` header excludes the transaction's representation; match the signing mode to what it accepts.
4. **PoP rejected with an `aud` mismatch** → `OID4VC_PUBLIC_URL` isn't the URL the wallet actually reached.
5. **`holderBinding` fails** → the wallet presented with a key other than the one it proved possession of at issuance.
6. **`dcql` fails, everything else `OK`** → valid credential, wrong disclosure. The crypto is fine; the query or the wallet's claim selection isn't.
