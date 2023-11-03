---
description: Release notes and artifacts
---

# Releases

{% tabs %}
{% tab title="Release Notes" %}
### v0.0.14 (30 June, 2023)

* Add Bulk issuance services
* Add Digilocker Certificate API services
* Customization of QR Code and PDF
* Metrics service
* Configuring passwords in schema definitions
* Credential Revocation
* SSL support for Elastic Search
* Notifications when an entity is create, updated or invited
* Helm charts for Sunbird-RC installation
* Keycloak update
* Elastic Search update
* Unique index creation for nested fields in entities
* Assorted bug fixes


{% endtab %}

{% tab title="Artifacts" %}
The various artifacts that form the crux of the Sunbird-RC platform are as follows. All of Sunbird-RC's artifacts are hosted on the Github Container Registry.&#x20;

| Artifact               | Image                                                    |
| ---------------------- | -------------------------------------------------------- |
| Sunbird-RC Core        | ghcr.io/sunbird-rc/sunbird-rc-core                       |
| Claims services        | ghcr.io/sunbird-rc/sunbird-rc-claims-ms                  |
| Signer services        | ghcr.io/sunbird-rc/sunbird-rc-certificate-signer         |
| Certificate services   | ghcr.io/sunbird-rc/sunbird-rc-certificate-api            |
| Notification services  | ghcr.io/sunbird-rc/sunbird-rc-notification-service       |
| Digilocker services    | ghcr.io/sunbird-rc/sunbird-rc-digilocker-certificate-api |
| Metrics services       | ghcr.io/sunbird-rc/sunbird-rc-metrics                    |
| Bulk issuance services | ghcr.io/sunbird-rc/sunbird-rc-bulk-issuance              |
| Public Key services    | ghcr.io/sunbird-rc/sunbird-rc-public-key-service         |
| Context Proxy services | ghcr.io/sunbird-rc/sunbird-rc-context-proxy-service      |
| Keycloak               | ghcr.io/sunbird-rc/sunbird-rc-keycloak                   |

The image tag **`latest`** always points to the latest version of the platform. One can also use the version number as the image tag. The current version of the platform is **`v0.0.14`**
{% endtab %}
{% endtabs %}



For a comprehensive list of changes in all our releases, you can find the detailed changelog available at this link. [https://github.com/Sunbird-RC/sunbird-rc-core/releases](https://github.com/Sunbird-RC/sunbird-rc-core/releases)

###



