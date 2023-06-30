# Organ Registries

Organ Registries are designed to facilitate the coordination and management of organ donation and transplantation. These registries serve as centralized databases that store information about both donors and recipients, allowing for efficient matching and allocation of organs.

Most importantly, this information leads to better matches between donors and recipients that ultimately ensures successful transplantation operations, saving thousands of lives annually.&#x20;

Organ Registries are the transformative solution that healthcare providers and transplant organizations can utilize to streamline their processes. Through leveraging this technology, the prospects of successful transplantation procedures rise dramatically along with positive patient outcomes. The bottom line is that Organ Registries aid in effectively governing organ transfers by achieving heightened coordination and minimized errors that translate into maximized saving of lives.

## Sunbird RC Reference Solution

The Sunbird RC reference solution is a means for systems to quickly kick-start their Registry journey, allowing for easy deployment of Registries and related processes. The reference solution provides various capabilities in a ready-to-use format, and adopters can quickly deploy these in their specific context with minimal configurations. Various code modules can be stitched together to enable workflows as desired, giving adopters the flexibility to design their own systems as per their needs.&#x20;

Organ Registry is a sample (a reference solution) built on Sunbird RC demonstrating registry of the available donors in the country who have pledged to donate their organs and enable verification of authentic recipients seeking organ transplant so that unnecessary or fraudulent organ requests can be avoided.

The organ registries are facilitated with the capability to issue verifiable digital pledge certificates that take input from the stakeholder’s registration data (maintained by the registry component).\


## Type of Organ Registries

* Pledge Registry
* Live Donor Registry (WIP)
* Recipient Registry (WIP)
* Cadaver Registry (WIP)

### Pledge Registry

Citizens can pledge to donate their organs once they are declared brain stem dead.&#x20;

#### User Persona for Pledge Registry

1\.     Citizen (Pledger)

2\.     Issuing Authority



## Capabilities Enabled

<figure><img src="../../../.gitbook/assets/image (1) (3).png" alt=""><figcaption></figcaption></figure>

* User Onboarding
* Issuance of Credentials: Generation, Updation and Revocation services
* Capability for the recipient to login to the registry and view, download and share the credentials.
* VC Verification&#x20;

## Reference Solution Components

### Registry&#x20;

### [Notification Service](https://docs.sunbirdrc.dev/learn/readme/high-level-architecture#notification-ms)

#### Source Code

{% embed url="https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/services/notification-service" %}

#### Configuration&#x20;

{% embed url="https://docs.sunbirdrc.dev/developer-documentation/configuration#notification-service" %}

### [Certificate Issuance](https://docs.sunbirdrc.dev/reference-solutions/certificate-issuance)[ ](https://docs.sunbirdrc.dev/reference-solutions/certificate-issuance)

#### Source Code

{% embed url="https://github.com/Sunbird-RC/demo-certificate-issuance" %}

### [Certificate Signer](https://docs.sunbirdrc.dev/learn/readme/high-level-architecture#certificate-signer)

#### Source Code

{% embed url="https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/services/certificate-signer" %}

#### Configurations

{% embed url="https://docs.sunbirdrc.dev/developer-documentation/configuration#certificate-signer-service" %}

### [Certificate Presentation Service](https://docs.sunbirdrc.dev/learn/readme/high-level-architecture#certificate-api)

#### Source Code

{% embed url="https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/services/certificate-api" %}

#### Configuration&#x20;

{% embed url="https://docs.sunbirdrc.dev/developer-documentation/configuration#certificate-api-service" %}

## Demo/Sandbox Site link

Use case implementation can be found here at:

{% embed url="http://demo-donor-registry.xiv.in/" %}





\
