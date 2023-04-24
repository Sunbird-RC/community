# Education Ecosystem

Education Ecosystem Registries (EER) are being envisioned as a means to empower end users with capabilities to manage their data and learning/skilling credentials in a digital format - allowing for increased trust at lower costs, portability, ease of access and use as well as consented and controlled sharing. This will allow the entire ecosystem to up level, seamlessly leveraging benefits that technology offers.  &#x20;

Registry usage can be applied to various use-cases that are cumbersome as of today - these include certification, admissions, transfers, accreditation, awards and recognitions etc. Such processes today often take significant time and effort, and are also riddled with issues such as lack of trustworthy documents, duplication etc. Registries that enable sharing of verifiable and trustworthy documents with a single click can greatly reduce the complexity of several such processes, anchoring all transactions on a single source of truth for the documents, and allowing users to manage them. Users can also be allowed to manage their data and request corrections to their data if required - thus ensuring they do not have to deal with issues arising out of data mismatches across different systems.  Consented and seamless data exchange between systems via Open APIs also eliminates errors and duplication, and can allow for easy, single-click onboarding of users.  &#x20;

The Indian education and skilling sector is rapidly evolving, and there is a fast-growing ecosystem of private as well as government agencies that are capable of providing students and individuals with multiple offerings including job opportunities, scholarships, training courses, certifications etc. While the intent here is to ensure that deserving individuals get equal opportunities, making this practically possible is still challenging. Discovery of benefits by end users, being able to identify deserving candidates for such offerings, lack of trustworthy information in a consolidated format, mismatch of demand and supply etc. all add to issues that prevent the processes from being effective and achieving their goal.

Policymakers and ecosystem players and hence now bringing in more focus on creating high trust environments at a lower cost - and also streamlining of resources, enabling easy discovery, simplified verification, greater user control over their own data etc. Registries can become powerful force multipliers for existing systems, helping ease out several issues that they face today and allowing them to leap-frog into a digital environment that allows for high trust at low costs. This can hugely benefit the learning and employable population of the nation as they participate in various learning, vocational and employment transactions over their lifetime.

## Sunbird RC Reference Solution:

The Sunbird RC reference solution is a means for systems to quickly kick-start their Registry journey, allowing for easy deployment of Registries and related processes. The reference solution provides various capabilities in a ready-to-use format, and adopters can quickly deploy these in their specific context with minimal configurations. Various code modules can be stitched together to enable workflows as desired, giving adopters the flexibility to design their own systems as per their needs.&#x20;

This reference solution has been designed to enable the needs of potential RC adopters - there are use cases being taken up which require issuers (who already have recipient data in some form - DB / Registry) to be able to issue VCs and end users to be able to see it via their DigiLocker. The various components of the reference solution can be used to enable such capabilities, irrespective of how the workflow specifics are tailored.

### Capabilities enabled

* Issuance of credentials (by Issuer admin) - for a single user/ in bulk mode
* Configure various templates for credentials via backend APIs
* Basic issuer dashboards on the issuance portal that keep track of the credentials issued
* The ability for Issuer to register on DigiLocker (to allow the end user to pull their credentials from the issuer onto their DigiLocker)
* Capability for the recipient to log in to the Issuer portal and view/ download/ share their credentials
* The ability for recipients to pull their credentials from the issuer onto their DigiLocker account
* Data emit capability for RC + sample dashboards that leverage the data being emitted
* Generic SSO capability using MeriPehechaan that any adopter can choose to leverage if required: the capability to carry out SSO-based login + profile sharing (including Aadhaar token) using DigiLocker - onto a portal

### Reference solution Components

**Issuance Portal**: [https://docs.sunbirdrc.dev/reference-solutions/certificate-issuance](https://docs.sunbirdrc.dev/reference-solutions/certificate-issuance)

**Certificate signer**: [https://docs.sunbirdrc.dev/learn/readme/high-level-architecture#certificate-signer](https://docs.sunbirdrc.dev/learn/readme/high-level-architecture#certificate-signer)

* Source Code: [https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/services/certificate-signer](https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/services/certificate-signer)
* Configurations: [https://docs.sunbirdrc.dev/developer-documentation/configuration#certificate-signer-service](https://docs.sunbirdrc.dev/developer-documentation/configuration#certificate-signer-service)

**DigiLocker SSO**: [https://docs.sunbirdrc.dev/use/sso-with-existing-systems/digilocker-meripehchaan-sso](https://docs.sunbirdrc.dev/use/sso-with-existing-systems/digilocker-meripehchaan-sso)

**Notification service**: [https://docs.sunbirdrc.dev/learn/readme/high-level-architecture#notification-ms](https://docs.sunbirdrc.dev/learn/readme/high-level-architecture#notification-ms)

* Source Code: [https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/services/notification-service](https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/services/notification-service)
* Configurations: [https://docs.sunbirdrc.dev/developer-documentation/configuration#notification-service](https://docs.sunbirdrc.dev/developer-documentation/configuration#notification-service)

**KeyCloak**: [https://docs.sunbirdrc.dev/learn/readme/high-level-architecture#certificate-api](https://docs.sunbirdrc.dev/learn/readme/high-level-architecture#certificate-api)&#x20;

* Configurations: [https://www.keycloak.org/server/configuration](https://www.keycloak.org/server/configuration)

**Certificate/Presentation service**: https://docs.sunbirdrc.dev/learn/readme/high-level-architecture#certificate-api[^1]

* Source Code: [https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/services/certificate-api](https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/services/certificate-api)
* Configurations: [https://docs.sunbirdrc.dev/developer-documentation/configuration#certificate-api-service](https://docs.sunbirdrc.dev/developer-documentation/configuration#certificate-api-service)

**Dashboard & Metrics**:

Sunbird RC allows for the 'emit' of events, which can be used to observe what is happening within the RC system. The system can be configured to emit events for actions such as ADD/ EDIT/ DELETE of credentials, or other actions carried out. Any adopter can configure the system to emit events that are required for the generation of metrics of interest. The followings are the links to the code and the configs for the RC dashboard

* Emission of Metrics: [https://docs.sunbirdrc.dev/developer-documentation/metrics](https://docs.sunbirdrc.dev/developer-documentation/metrics)
* Configurations: [https://docs.sunbirdrc.dev/developer-documentation/configuration#metrics-service](https://docs.sunbirdrc.dev/developer-documentation/configuration#metrics-service)&#x20;
* Source code: [https://github.com/varadeth/sunbird-rc-core/tree/metric\_service/services/metrics](https://github.com/varadeth/sunbird-rc-core/tree/metric\_service/services/metrics)

<figure><img src="../../../.gitbook/assets/screenshot-demo-education-registry.xiv.in-2023.04.21-18_58_45.png" alt=""><figcaption><p>Example Dashboard</p></figcaption></figure>

[^1]: 

