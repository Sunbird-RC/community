# Core Capabilities

<figure><img src="https://user-images.githubusercontent.com/580711/195899958-d88fd936-2a91-424c-9f92-ed86613b9f9a.png" alt=""><figcaption></figcaption></figure>



## Registry

A governing body/authority would be able to build registry that acts as a single source of truth

1. Define Schema as per policy:
   * Define field and field types
   * Field level privacy: Able to define visibility of who can view the content at field level
   * Consent framework at schema level
2. Create entities:
   * Able to Bulk invite users via CSV
   * Able to link to external systems
   * Able to self-register
   * Able to decide visibility of who can view the content at field level
3. Define Ownership
   * Able to define which entities can login & how is authorisation handled
4. Discovery: Able to define discovery attributes that are public, private or consent based
5. Analytics: Basic Analytics for Registries

## Verifiable Credentials (VC)

1. Define Attestation and Claim Workflow: Tenants can define the 'add on' workflow
   * Able to configure how claims will be approved (Auto or Manual)
   * Using Internal (Sunbird RC system) or Connecting to external systems
   * Able to create multi-level attestation for claim
   * Define Validity
2. Consent:
   * Grant or Revoke Consent: Able to receive consent requests to grant or revoke the consent access.
   * Consent Auditing
3. Issuance services:
   1. W3C compliant&#x20;
   2. Create and Issue Verifiable Credentials
   3. Updation and Revocation
4. Unique ID generation

## VC Verification

1. Offline verification of Verifiable Credentials: [Reference SDK](https://docs.sunbirdrc.dev/vc-verification-module)
2. Consent based access: Able to provide consent to another system/person to access Verifiable Credentials for the purpose of transaction/interaction

## Digital Wallet

Citizen (credential owners/holders) can access Verifiable Credentials anytime anywhere.

1. Fetch and store personal verifiable credentials
2. Consent based sharing of verifiable credentiala
