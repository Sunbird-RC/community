# Releases

### Release Notes

#### v0.0.14 (30 June, 2023)

* Implemented the capability to issue Verifiable credentials in bulk directly from CSV files.
* Added support for pushing Verifiable credentials to Digilocker for enhanced accessibility.
* Introduced the option to customize both QR Codes and PDFs for a more tailored experience.
* Included a metrics service to generate Sunbird telemetry-compatible events, capturing entity audit events efficiently.
* Provided the ability to configure password generation when issuing a Verifiable credential, facilitating entity owner access.
* Enabled support for the revocation of verifiable credentials, enhancing security management.
* Implemented SSL support for Elastic Search to ensure secure data storage.
* Added the feature to generate notifications when entities are created, updated, or invited, improving user engagement.
* Introduced Helm charts for straightforward Sunbird-RC installation.
* Upgraded the Keycloak image to support nonce for better security.
* Updated Elastic Search libraries in the registry to enhance performance and compatibility.
* Configured and generated unique index creation for nested fields in entities, ensuring data integrity.
* Addressed various bug fixes to enhance the overall user experience.

For a comprehensive list of changes in all our releases, you can find the detailed changelog available at this link. [https://github.com/Sunbird-RC/sunbird-rc-core/releases](https://github.com/Sunbird-RC/sunbird-rc-core/releases)

### Artifacts

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

The **`latest`** tag always points to the latest version of the platform. The current version is also tagged to the image. The current version is **`v0.0.14`**

The details about the various docker images are available at: [https://github.com/orgs/Sunbird-RC/packages?repo\_name=sunbird-rc-core](https://github.com/orgs/Sunbird-RC/packages?repo\_name=sunbird-rc-core)
