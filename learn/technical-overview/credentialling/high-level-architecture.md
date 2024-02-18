# High-Level Architecture

Sunbird-RC's credentialling service is an amalgamation of three microservices.

* **Identity Microservice:** This service is the central lynchpin to maintain identities across the system. All identities in Sunbird-RC are DID-compliant and are web-resolvable. The other microservices (Credential Schema Service and Credential Service) depend on this for generating any identity.
* **Credential Schema Microservice:** This service stores the schema of the Verifiable Credential along with the associated view template.
* **Credential Issuance Microservice:** This is the core issuance service. This service is called with the payload, which is then transformed into a W3C-compliant Verifiable Credential in JSON-LD format.  The payload is then signed using the private key which was generated as part of the original Issuer creation.&#x20;

### Architecture Diagram



### Sequence Diagram

```mermaid
---
title: Sequence Flow
---
sequenceDiagram
    actor callingSystem as Calling<br/>System
    participant identity as Identity<br/>Service
    participant schema as Credential<br/>Schema
    participant credential as Credential<br/>Issuance<br/>Service
    participant vault as Hashicorp<br/>Vault
    participant db as Database
    
    autonumber
    loop Creating an Issuer Identity
    callingSystem->>+identity: Create an Identity<br/>for the Issuer
    identity->>identity: Generate Public and Private Key<br/>for an identity
    identity-->>vault: Persist the Key Pair<br/>into the vault
    identity-->>db: Persist into the database
    identity-->>-callingSystem: Returns a DID compliant<br/>Identity
    note over callingSystem,identity: The DID document contains<br/>Identity metadata with<br/>the associated public key
    end
    
    loop Creating a credential Schema and template for presentation
    callingSystem->>+schema: Create Schema
    schema->>+identity: Create an identity<br/>for the schema
    identity->>identity: Generate a Key Pair
    identity-->>vault: Persist Key Pair to Vault
    identity-->db: Persist Identity in database
    identity-->>-schema: Return DID compliant Identifier
    schema-->db: Persist Schema into Database
    schema-->>-callingSystem: Return schema Identifier 
    callingSystem->>+schema: Add template to schema
    schema-->>db: Add Template to Schema
    schema-->>-callingSystem: Return status
    note over callingSystem, schema: Credential Schema creation is mandatory for Credential Issuance.<br/>The payload for the credential is validated against the credential schema.<br>The payload is  transformed into the View Template.
    end
    
    loop Generating a Verifiable Credential
    callingSystem ->>+credential: Request to Issue a credential
    credential-->>+schema: Request to Retrieve associated credential schema
    schema-->>-credential: Retrieve associated schema
    credential->>+identity: Request Issuer Identity
    identity->>+vault: Request Keys for Issuer
    vault->>-identity: Retrieve Keys for Issuer
    identity-->>-credential: Retrieve Issuer DID with Keys
    credential->>credential: Validate payload against Schema<br/>and convert to JSON-LD W3C-VC data model
    credential->>credential: Sign payload and add proof
    credential-->>-callingSystem: Issue W3C compliant Verifiable Credentials
    end
    
    loop Verify a Sunbird-RC issued Verifiable Credential
    callingSystem->>+credential: Request to verify a Verifiable Credential
    credential->>+identity: Request to retrieve the DID document for the issuer
    identity-->>vault: Retrieve public Key of the Issuer
    identity-->>-credential: DID Document of the issuer including the public key
    credential->>credential: Validate the proof section of the VC<br/>using Issuer's public key
    credential->>credential: Validate against revocation list
    credential-->>-callingSystem: Respond with the Verification status
    end
    
 
```

