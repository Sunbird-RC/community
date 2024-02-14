# Introduction

All digital platforms require master data and actor (person/entity/thing) data related to that system be maintained for identification, validation, etc. For example, a property tax system needs to maintain master data about properties, land boundaries, tax codes, tax payers, inspection officers, etc. in a structured and validated fashion so as to help manage the property tax transaction in a seamless manner.

As the world becomes data rich, it is essential that various data about people, entities, geographies, resources, assets, etc. are made available in electronic registries with Open APIs for other applications to seamlessly validate and use attested and authenticated data. This is even more critical when it comes to people and entities where various claims can be electronically validated against such registries via open APIs avoiding paper-based validations, thus increasing trust while decreasing cost of validation.

Traditionally such data is owned and maintained within registers and kept frequently updated. But there are three big issues that are very common among most of these systems;

* **LIVE**: Due to its changing nature, such data often goes stale (not up-to-date), thus increasing the cost of collection and maintenance. For example, information about schools and teachers, their contact details, etc. get outdated, forcing departments to redo data collection every few years, and digitise, through time consuming processes.
* **REUSEABLE**: Even if such data is maintained by one system, it is not available to be reused by other systems forcing every department needing such data to repeat data collection and maintenance.
* **TRUSTWORTHY**: When data is exchanged between systems, trust of that data is established by ensuring the entire record itself is digitally signed and the fact that registry record comes with attestations along with the data. For example, a list of schools downloaded as a CSV file from a portal cannot be trusted by other systems since there is no guarantee that it is authentic and has not been edited subsequently.

The government, educational industry, and other ecosystems issue certificates, licenses, etc. Availability of these in paper form creates issues of low trust, lack of credibility, information asymmetry, time consuming physical verification, and non-portability amongst many others. Additionally, it is difficult to verify the legitimacy of these documents. Verification is a time-consuming and challenging procedure that frequently takes weeks or even months to complete. Moreover, Users have very little control over who has access to their personal information, where their papers are stored, and how they are used.

[Click here](community/comprehensive-overview-electronic-registries-and-verifiable-credentials/) to learn more about electronic registries and verifiable credentials

#### _An interoperable and unified registry infrastructure needs to be built to enable "live", "reusable", and “trustworthy” registries as a “single source of truth” to address the above three core issues._

**Sunbird RC** is a "low code" framework to enable organizations to rapidly build next generation electronic registries and verifiable credentials. Sunbird RC uses a set of configurations to rapidly build out registries, automatically generate CRUD (create/ read/ update/ delete) APIs without any coding, enable registry searches and access via open APIs, issue and manage verifiable credentials, manage user consent flows if required, manage attestation and verification flows, etc.

Sunbird RC is listed as a global Digital Public Good (DPG) within the [Digital Public Good Alliance (DPGA) registry](https://digitalpublicgoods.net/registry/). It meets the DPG standard of privacy and other applicable best practices, does no harm by design, and is of high relevance for attainment of the United Nations 2030 Sustainable Development Goals. Sunbird RC is the core engine within [DIVOC](https://divoc.dev/), a globally recognized DPG for vaccination and health credentialing. Sunbird RC is also part of India's massively adopted [DIKSHA](https://diksha.gov.in/) school education platform used at population scale.

Sunbird RC is open sourced under [MIT license](https://opensource.org/licenses/MIT). Any individual or organisation can leverage this building block for free for their purposes in accordance with the MIT license. We strongly encourage you to participate in the community and contribute back to help improve this project.

_**“This website abides by the**_ [_**Terms of Use**_](https://sunbird.org/terms-conditions) _**and**_ [_**Privacy Policy**_](https://sunbird.org/privacy-policy) _**of**_ [_**www.sunbird.org**_](http://www.sunbird.org)_**.”**_

_“Copyright © 2021 EkStep Foundation. This work is licensed under a_ [_Creative Commons Attribution (CC-BY-4.0) International License_](https://creativecommons.org/licenses/by/4.0/) _unless otherwise noted.”_
