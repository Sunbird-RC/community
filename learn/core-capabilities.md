# Core Capabilities (WIP)
<img width="600" alt="Screenshot 2022-10-14 at 7 40 58 PM" src="https://user-images.githubusercontent.com/580711/195899958-d88fd936-2a91-424c-9f92-ed86613b9f9a.png">


## Registry
A governing body/authority would be able to build registry that acts as a single source of truth
1. Define Schema as per policy:
    1. Define field and field types
    2. Field level privacy: Able to define visibility of who can view the content at field level
    3. Consent framework at schema level
3. Create entities: 
    1. Able to Bulk invite users via CSV
    2. Able to link to external systems
    3. Able to self-register
    4. Able to decide visibility of who can view the content at field level 
4. Connecting to external systems
5. Define Ownership
    1. Able to define which entities can login & how is authorisation handled
6. Discovery: Able to define discovery attributes that are public, private or consent based
7. Consent: 
    1. Grant or Revoke Consent: Able to receive consent requests to grant or revoke the consent access.
    2. Consent Auditing
8. Analytics: Basic Analytics for Registries

## Verifiable Credentials (VC)
1. Attestation and Claim:
Tenants can define the 'add on' workflow 
    1. Able to configure how claims will be approved
    2. Able to create multi-level attestation for claim
    3. Validity
2. Issuance services:
    1. Create and Issue Verifiable Credentials
    2. Update
    3. Revoke
3. Unique ID generation

## VC Verification
1. Offline verification of Verifiable Credentials: [Reference SDK](https://docs.sunbirdrc.dev/vc-verification-module) 
2. Consent based access: Able to provide consent to another system/person to access Verifiable Credentials for the purpose of transaction/interaction

## Wallet
Citizen (credential owners/holders) can access Verifiable credential anytime
