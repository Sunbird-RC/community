# Core Capabilities

<figure><img src="../../.gitbook/assets/Artboard 3 (2).png" alt=""><figcaption></figcaption></figure>

### Registry

A governing body/authority would be able to build registry that acts as a single source of truth

1. **Define Schema as per policy:**

* Define field and field types: Can create new fields in the registry to store specific types of information and can also specify the type of data that should be entered into each field. For example: if an adopter is building a student registry, they may define the fields for student name, father's name, mother's name, school name, and age, and may define that the age field should accept only numeric.
* Field level privacy: This feature allows the individuals/organizations to define different levels of privacy for different fields based on the sensitivity of the data contained within them. For example, in a patient registry, adopters may want to restrict access to certain fields containing patient health information to only authorized healthcare professionals, while other fields containing less sensitive information such as patient demographics may be accessible to a wider range of users.
* Consent framework at schema level

2. **Create entities:**

* Able to Bulk invite/onboard users via CSV
* Able to link to external systems
* Able to self-register
* Able to decide visibility of who can view the content at field level

3. **Define Ownership**

* Able to define which entities can login & how is authorization handled

4. **Discovery**: The visibility of discovery attributes can be controlled through access control mechanisms such as consent-based sharing, which allows users to control who can view their information. Specifically, users can define which discovery attributes are public (visible to all users), private (visible only to the user), or consent-based (visible to selected users or groups based on their consent).
5. **Analytics**: Basic Analytics for Registries

### Verifiable Credentials (VC)

1. **Define Attestation and Claim Workflow**:

* Tenants (users) can define an 'add-on' workflow, which means they can customize the steps in the workflow to suit their needs. Additionally, they can configure how claims will be approved, either automatically or manually.
* The system used for attestation can be an internal system like Sunbird RC or an external system that the tenant connects to.
* Multi-level attestation for a claim can be created i.e. the claim can be reviewed by multiple parties or levels of approval.
* Define Validity

2. &#x20;**Consent:**

* Grant or Revoke Consent: Able to receive consent requests to grant or revoke the consent access.
* Consent Auditing

3. **Issuance services**

* W3C compliant
* Create and Issue Verifiable Credentials
* Updation and Revocation
* Unique ID generation

### VC Verification

* Offline verification of Verifiable Credentials:[ Reference SDK](https://docs.sunbirdrc.dev/vc-verification-module)
* Consent based access: Able to provide consent to another system/person to access Verifiable Credentials for the purpose of transaction/interaction.

### Digital Wallet

Citizen (credential owners/holders) can access Verifiable Credentials anytime anywhere.

1. Fetch and store personal verifiable credentials :With digital wallet, individuals can access their credentials anytime anywhere as the wallet can fetch and store the issued credentials from various entities.
2. Consent based sharing of verifiable credentials:The digital wallet also enables consent-based sharing of verifiable credentials with third parties, such as employers, service providers, or other organizations that require proof of identity ,qualifications, association etc. This means that the individuals can control who has access to their personal information and can choose to share only the necessary information for a specific purpose.
