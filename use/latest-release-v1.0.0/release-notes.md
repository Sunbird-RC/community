# Registry

<table data-full-width="false"><thead><tr><th>Release Version</th><th>Date</th></tr></thead><tbody><tr><td>1.0.0</td><td>15-Nov-2023</td></tr></tbody></table>

* **Full OAuth and OIDC support -** Sunbird-RC was dependent on Keycloak for its Identity and Access Management needs. Sunbird-RC has been enhanced to support any OAuth 2.0 and OIDC-compliant provider.
* **Enhanced Data Privacy with Encryption & Decryption on the Private Fields -** Sunbird-RC has been enhanced to encrypt, mask or hash data at a field level even during storage. Earlier, data was encrypted and masked only when shared with other integrators.
* **Unique ID Generation for Fields -** Sunbird-RC has incorporated the capability to generate and add functional identifiers in registries, moving away from generic UUIDs. This enhancement facilitates seamless integration with DIGIT's Id-Gen service.
* **Cleanup of Registry Startup Errors -** Users reported various startup issues, especially when authentication is disabled or related to keycloak, elastic search, and auxiliary services. The update addresses these concerns for a smoother startup.
* **Enhanced Registry-CLI -** Registry-CLI has been upgraded and new options have been added for enabling or disabling various features. These updates empower users to tailor the CLI to their specific needs and streamline its operation based on individual use cases.
* Migrated all Registry Core Docker images from Dockerhub to the GitHub Container Registry (GHCR)
* Various bug fixes to enhance the overall user experience.

For a comprehensive list of changes in all our releases, you can find the detailed change log available at this link. [https://github.com/Sunbird-RC/sunbird-rc-core/releases](https://github.com/Sunbird-RC/sunbird-rc-core/releases)

### Artifacts

The various artifacts that form the crux of the Sunbird-RC platform are as follows. All of Sunbird-RC's artifacts are hosted on the Github Container Registry.

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
| ID Generation services | ghcr.io/sunbird-rc/id-gen-service                        |
| Encryption services    | ghcr.io/sunbird-rc/encryption-service                    |

The **`latest`** tag always points to the latest version of the platform. The current version is also tagged to the image. The current version is **`v1.0.0`**

The details about the various docker images are available at: [https://github.com/orgs/Sunbird-RC/packages?repo\_name=sunbird-rc-core](https://github.com/orgs/Sunbird-RC/packages?repo\_name=sunbird-rc-core)
