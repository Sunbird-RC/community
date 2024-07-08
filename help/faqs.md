# FAQs

## General

**Who is responsible for developing and maintaining Sunbird RC?**

Sunbird RC is available under the MIT open-source license. All components included in Sunbird RC are developed as open-source and are freely accessible to anyone across the globe. This project is maintained by EkStep Foundation, India.

**Can an entity self-host it ? What support is available to facilitate the process?**

Yes, It is possible for the entities to host Sunbird RC on their own, thereby retaining complete control over the data. The implementation of Sunbird RC can be completed in just 6 to 8 weeks, and consulting and system integrator organizations can be engaged to assist with the installation, configuration, operation, monitoring, and management of the system. The detailed [documentation](https://docs.sunbirdrc.dev/learn/readme) provided outlines the necessary steps and configurations required for setting up and implementing Sunbird RC.

**How can countries/organizations benefit from this?**

Sunbird RC offers entities the freedom to utilize, manage, and distribute the platform as they wish, without any restrictions on modifications or distribution. This flexibility allows entities to tailor the software to their specific needs and deploy it at scale to benefit their citizens/users.

**Can we use only Verifiable Credentials component of Sunbird RC?**

Yes, it is possible as credentialing is a specific and independent microservice and it can function autonomous from the registries.

**What makes Sunbird RC verifiable credentials secure and tamper-proof?**

Two methods have been implemented in the underlying architecture to ensure the security and integrity of the QR code. The first method involves compressing the QR data payloads using popular compression models to make the QR size manageable for wider readability among verifier applications. Verifier applications are required to decompress the signed QR payloads in order to access the message content. The second method involves digitally signing the QRs using cryptographic keys generated via a PKI technology that uses RSA or ECDSA algorithms. Adopter countries have the option to determine the distribution channels of the "public key" with verifiers to enable them to verify the authenticity of the signed QR code. These two approaches provide the necessary layer of security to prevent unauthorized entities from tampering with the QR content.

**Can the credentials issued by Sunbird RC be in multiple languages?**

Yes, Sunbird RC supports the issuance of credentials in multiple languages. Organizations/Countries can provide their citizens with credentials that are localized to their language of choice.

**Are the credentials issued by Sunbird RC usable offline?**

Yes, the credentials issued by Sunbird RC are usable offline.

**Does Sunbird RC support bulk issuance of credentials via file upload?**

To facilitate the issuance of credentials in bulk, a dedicated bulk API has been developed that enables organizations to issue a large number of credentials at once. This API is not directly integrated with Sunbird RC, but instead, we have built a middleware or BFF (backend for frontend) service that acts as an intermediary between the bulk API and Sunbird RC.

**How much time it takes to get a test instance of RC up and running it?**

If you already have the necessary schemas prepared, configuring RC should be a relatively quick process, possibly taking only a few hours. The docker compose file is readily accessible for this purpose.

The process of defining the schemas is similar to designing a database or entity-relationship model. It involves starting with identifying the entities and their relationships, determining the actions or flows that need to be supported, and ultimately specifying the fields for each entity.

**Do the private fields get stored as encrypted format?**

We can encrypt the private data, but the encryption service is not provided by default in the package.

**Is there a sandbox environment available for testing the Sunbird RC registry engine and API handshakes?**

Yes, Sunbird RC provides a number of sandboxes and demos that are available for testing and familiarizing with the platform's capabilities. These sandboxes can be found in the "SAMPLES" section of the Sunbird RC documentation website and include a variety of use cases and scenarios.

One of the available samples is the [education sample](https://docs.sunbirdrc.dev/sample-use-cases/edu-registries). This sample is designed to showcase how Sunbird RC can be used in an education context.

**Do we have the deployment scripts for running the instance in a Kubernetes cluster?**

Yes, Sunbird RC provides deployment scripts that can be used to run the platform in a Kubernetes cluster. You can access the deployment scripts at [https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/infra](https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/infra).These scripts are designed to be easy to use and provide a streamlined deployment process for running Sunbird RC in a Kubernetes environment.

**How are the security and privacy concerns handled when working with verifiable identity?**

Sunbird RC uses Verifiable Credentials (VCs) which are signed by asymmetric keys, ensuring that only the public key is shared publicly. This provides a high level of security and privacy, as the private key is kept confidential and is used only by the credential holder to prove their identity when required.

**How can I follow updates?**

Sunbird RC provides multiple channels through which you can follow updates and stay informed about the latest developments. These channels include:

* **GitHub**: Sunbird RC's [GitHub repository](https://github.com/orgs/Sunbird-RC/repositories) is the primary source for technical and project management updates. You can follow the repository to receive notifications about new releases, bug fixes, and other technical updates.
* **Social accounts**: Sunbird maintains active social media accounts on [Twitter](https://twitter.com/Sunbird\_EkStep) and [LinkedIn](https://www.linkedin.com/company/sunbirdbuildingblocks/). By following these accounts, you can receive regular updates and news about the platform, as well as insights from the Sunbird team.
* **GitHub Discussions and Discord**: Sunbird RC has active discussion forums on [GitHub](https://github.com/Sunbird-RC/community/discussions) and [Discord](https://discord.gg/Q5mvw2mGC8), where community members can engage with one another and discuss topics related to the platform. These forums are a great way to stay connected with the Sunbird RC community and get involved in ongoing discussions.

**How can I contribute/participate?**

Sunbird RC welcomes contributions from the community. There are several ways in which you can contribute, including:

**Reporting bugs/requesting features**: If you encounter any issues while using Sunbird RC or have suggestions for new features, you can [report ](https://github.com/sunbird-rc/community/issues/new/choose)them on the project's GitHub repository. This helps the Sunbird RC team identify and address issues more quickly.

**Contributing to the core registry:** If you have experience with software development, you can contribute to the core registry by submitting bug fixes, code improvements, or new features. You can do this by submitting a pull request on the Sunbird RC GitHub repository.

**Contributing to the registry CLI**: If you have experience with CLI development, you can [contribute to the registry CLI ](https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/tools/cli/contributing.md)by submitting code improvements or new features.

**Improving documentation:** Another way to contribute to Sunbird RC is by improving the documentation. This can include updating existing documentation, adding new sections, or improving the clarity and accuracy of the documentation.

#### How do i get started ? Which services should i start and how? Which repository should i use for my use-case?

We should understand the Sunbird registry and credentials first before jumping directly into applying it in a real use-case. we can follow the below roadmap to get started quickly -

1. Introduction - [link](https://rc.sunbird.org/)
2. Read about services and high level architecture -
   * [registry and other services](https://rc.sunbird.org/learn/technical-overview/registry/high-level-architecture)
   * [credentialling services architecture](https://rc.sunbird.org/learn/technical-overview/credentialling/high-level-architecture)
3. Write down your requirements and the feature you can leverage to using sunbird-rc
4. Understand how to configure a schema, micro level details are here - [link](https://rc.sunbird.org/use/developers-guide/functional-registry/schema-setup/schema-configuration)
5. Check the Api references for registry - [link](https://rc.sunbird.org/api-reference/registry-apis)
6. Try out deployments with default configurations to understand more, installation guides are there
   * If your requirement is only credentialing, you can checkout this - [link](https://github.com/Sunbird-RC/devops/tree/main/deploy-as-code/docker/v2/credentialling) , Also checkout this demo project -&#x20;
     * [demo-mosip-rc](https://github.com/Sunbird-RC/demo-mosip-rc) (it uses registry for only storing data only) - this has postman collections as well to try out. And one click installation - just start with `make start`
   * If your requirement is registry and credentialing use this - [link](https://github.com/Sunbird-RC/devops/tree/main/deploy-as-code/docker/v2/registry\_and\_credentialling)
7. Create your own schemas and then try
8. Search [here](https://rc.sunbird.org/?q=) if there are some issues or doubts
9. Still there are issues and not resolved, reach out to community discord or github discussions

#### What is the difference between a credential schema and a registry schema?

**Credential schema** - It is a JsonSchema of draft - \`[https://json-schema.org/draft/2020-12/schema](https://json-schema.org/draft/2020-12/schema)\`. It is created and managed using credential-schema service and used in credentials service to validate the credential subject while issuing a Verifiable Credential.

**Registry Schema** - It is a JsonSchema of draft - \`http://json-schema.org/draft-07/schema\`. It is used in registry service. It is responsible for -

1. Creating APIs for an entity to be managed in the registry.
2. Schema Configurations used in registry - is configured with key `_osConfig` , more details \[here]\([https://rc.sunbird.org/use/developers-guide/functional-registry/schema-setup/schema-configuration](https://rc.sunbird.org/use/developers-guide/functional-registry/schema-setup/schema-configuration))

#### What are the differences between registry release v1 and release v2?

There are a couple of difference and changes between v1 and v2 releases of registry. we can read more in the release notes on github though. The main difference is here -&#x20;

v2 release of registry introduced new signing and credentialing architecture design. v2 release is configurable to use v1 signing services or the new architecture. \
The env variables in registry, which can be used to configure or switch between the two versions -

* SIGNATURE\_PROVIDER `dev.sunbirdrc.registry.service.impl.SignatureV2ServiceImpl , dev.sunbirdrc.registry.service.impl.SignatureV1ServiceImpl`
* When using v2 version, make sure all the variables which has prefix `did_`, `signature_v2_` are provided in the registry

v2 has pagination support for List entity and Search entity APIs and have different response structure for these apis then v1

Both has difference in signing services and architecture -

| Release v1 and older                                                                               | Release v2                                                                                                                                                                            |
| -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Keys used to sign should be created manually and place them in a file config.json before deploying | Keys used to sign are created when issuer is created using identity service while signing using registry. the keys are stored in db and private key part is stored in hashicorp vault |
| To add new key, create and store them in the file manually and deploy                              | Keys are dynamically created while signing                                                                                                                                            |
| Are not very safe are private keys also stored in the same file                                    | Are safe as private key is stored in hashicorp vault                                                                                                                                  |
| Supports only one signature suite `RsaSignature2018`                                               | Has support for different signature suites `Ed25519Signature2020` , `Ed25519Signature2018` ,  `RsaSignature2018`                                                                      |
| Depends on services certificate-signer, certificate-api                                            | Depends on services identity, credential-schema, credentials and the vault                                                                                                            |

v2 support complete unique indexes and indexes creation

v2 has a lot more bug fixes which makes it easy to use without facing many errors.

v2 can be used to verify any verifiable credentials(with above signature types) then just issued by itself

Above are the main differences between two releases, checkout release notes for more details

#### Facing issues while creating an entity like "User is not allowed to perform the operation on this entity" or "Exception executing consequence for rule 'Create entity owner for newly added owner fields'" or&#x20;

## Registry

#### **Can an existing database, which gets routinely updated tho’ Business processes of department, be used as a source to create and dynamically update a sunbird powered registry?**

Yes, we can treat the Sunbird-registry as a master registry (a layer above the source databases), which is updateable (hence, “live” at all times) by any approved changes in the source databases. However, what constitutes an “update” and how should a “change in the source database” result in an “update” in the master-registry is determined by the business processes & rules established by the program. Technically, this is feasible.

#### Can excel sheet or other dBs be used as an one time source for creating a registry?

Yes, this can be done through scripts that allow bulk import of the source data into the registry.

#### Can we configure rules-based validation on data which comes into the registry either through API or data being input through a form?

Yes, since Sunbird RC supports defining a registry schema using a JSON Schema format, the properties can be defined with validations like mentioned here: https://json-schema.org/draft/2019-09/json-schema-validation.html#:\~:text=JSON%20Schema%20validation%20asserts%20constraints,descriptive%20metadata%20and%20usage%20hints.

#### Is there a facility to easily create a form for inputting data into registry followed by approval process?

Yes, the Sunbird RC UI supports the generation of dynamic forms, based on the schema created; as well as support creating “approval” workflows to aid the attestation & subsequent persistence of the “data” into the registry.

#### Can schema be re-defined (i.e., adding some extra data fields or removing certain fields)? If Yes, how versioning is managed and what happens to existing requests with old schema.

No, it doesn't support versioning now. This needs to be handled manually outside Sunbird RC 2.

#### Can validations be defined at field level, at the database level and also at the user interface level?

Yes, field and user interface level validations can be defined.

#### Does bulk upload of facilities create schema automatically, if yes all that one can do while creating schema be achieved through bulk upload also?

The schema needs to be created first, before seeding the data to the registry

**What databases can be used for Registry?**

The registry requires a main DB that is used as the main store for storing all data. The following DB providers can be used:

* Graph database (Neo4J)
* Relational databases (Postgresql, HSQLDB, H2, MariaDB, MySQL, MSSQLServer)
* NoSQL databases (Cassandra)

#### **The registry is throwing unauthorized issues (401) on retrieving entity. The registry throws "getaddrinfo ENOTFOUND keycloak" error.**

In dev/local setup, it is required that keycloak hostname is added to '/etc/hosts' file.

```bash
vi /etc/hosts
# add the below content to the above file.
...
127.0.0.1 keycloak
...
```

Use \`[http://keycloak:8080/auth/realms/sunbird-rc/protocol/openid-connect/token](http://keycloak:8080/auth/realms/sunbird-rc/protocol/openid-connect/token)\` url to fetch the token

API documentation for generating the token is available here, [authenticating-as-an-entity.md](../api-reference/registry-apis/registry/authenticating-as-an-entity.md "mention") [generate-admin-token.md](../api-reference/registry-apis/registry/generate-admin-token.md "mention")

If the above steps did not resolve the problem, please follow the next steps provided.

1. Goto the registry container / docker-compose and identify the value set for `sunbird_sso_url` . Copy the value for further user.
2. Goto keycloak admin console and select the realm Ex:`sunbird rc (if no new realm was created)` and goto `Realm Settings`
3. Paste the value copied in `Frontend URL` text box
4. Click on `Save`
5. If your url is not secure / doesn't have a SSL, goto `login` tab, select `none` for `Require SSL` and click on `Save`
6. Restart the registry

#### How can I create QR codes using VC in Sunbird RC?

* Firstly [certificate API service](../use/developers-guide/functional-registry/configuration/#certificate-api-service) should be configured to generate certificates.
* The certificate template should have the handlebars parameter `{{qrCode}}` to render a QR code in the certificate.
* Explore a sample template here - [svg](https://github.com/Sunbird-RC/demo-certificate-issuance/blob/main/sample.svg?short\_path=bd1d135) , [html](https://raw.githubusercontent.com/Sunbird-RC/demo-certificate-issuance/main/samples/TrainingCertificate.html)

_Additionally to know more about how to create a certificate template, please go through the documentation_ [_developers-guide/schema-setup/schema-configuration#certificate-template_](../use/developers-guide/functional-registry/schema-setup/schema-configuration.md#certificate-template)

## Ownership

#### Can citizens be allowed to login into the registry to view and suggest updation to his registry entry?

Yes, Sunbird-RC supports “user login” and also ownership management for a schema. Hence if the registry needs to have “citizen” access; it can be enabled. Just a point to reiterate, the purpose of a registry can be non-individual centric as well (in which case, the authorized “user”, can be institutional staff roles as well).

**Can we we use single sign on? or use any other authentication?**

Currently, Sunbird works on Keycloak OAuth. However, we can write a custom service to authenticate the user, with other authentication methods.

## Consent

#### Is consent given individually or can be given to a group of individuals or entities\*\*?\*\*

Currently, consent is given by an individual to the entities that he/she has ownership of (i.e. to the entities which were created by him/her). Consent can be given to one or more attributes that are part of the schema.

#### Can consent be revoked or made time bound or limited to some number of times?

Currently, consent can only be revoked manually. For time/count-based revocation of consent, we need to build a custom service to handle it.

## Attestation

#### Can we have automatic verification and attestation through existing databases?

Yes, attestation can be done with an external system. We need to write the functionality to enable data exchange between these two systems.

## **Postgres**

#### For what all purpose Postgres is used\*\*?\*\*

We have three services that uses Postgres\*\*.\*\*

1. Keycloak: Data related to user management, client management, realm management, consent etc will be stored
2. Registry core: All data created under the schemas will be stored.
3. Claims: All data related to attestation flows(claims) will be stored.

Sunbird RC can work on the below DBs:

1. Graph database (Neo4J)
2. Relational databases (Postgresql, HSQLDB, H2, MariaDB, MySQL, MSSQLServer)
3. NoSQL databases (Cassandra)

## Keycloak

#### How to setup keycloak with sunbird RC and what are the prerequisites needed?

You can check the following registry cli [https://docs.sunbirdrc.dev/developer-documentation/installation-guide.](https://docs.sunbirdrc.dev/developer-documentation/installation-guide.) All the prerequisites are mentioned here. Using this cli you will be able to bring up all the services required.\
The following keycloak configurations need to be configured in SB-RC.

[https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/docker-compose.yml#L36-L40](https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/docker-compose.yml#L36-L40)

#### **How to enable password based login in keycloak instead of otp based login?**

Currently, we have added custom SPI and themes to support otp based login in keycloak. If your usecase requires password based login, then the below changes need to be done in keycloak.\
**Changing the theme**\
1\. Login to keycloak admin console\
2\. Goto `Clients` > `registry-frontend`\
3\. Select `keycloak` in `Login Theme` option\
4\. Save the changes\
**Update login flow**\
1\. Login to keycloak admin console\
2\. Goto `Authentication` > `Bindings`\
3\. Select `browser` in `Browser Flow` option\
4\. Save the changes

#### How to fix the "401 unauthorized code" error when authentication enabled and providing the correct token?

1. Check what url host being used to generate a token
2. If you are not using http://keycloak:8080/auth then follow these steps -
   1. Change the keycloak frontend url using keycloak UI
   2. Login to the keycloak UI
   3. Select the realm - default `sunbird-rc`
   4. Go to `General Settings`
   5. Check for the field `frontend url`
   6. i.e., if you want to use `http://localhost:8080/auth` to generate token then set this value
   7. Recreate the registry container
   8. Regenerate a new token and use.
3. If you have fixed point 2, then check if the client credentials are valid -
   1. Login to keycloak UI and select the realm
   2. Go to `Credentials` tab
   3. Regenerate the secret and copy
   4. Use them as keycloak secret in the registry
   5. Env Variable exposed in docker-compose.yml is `KEYCLOAK_SECRET` which can be set in .env file and recreate the registry container

#### **How to fix the \`Role creation exception\` error in registry logs?**

In the registry, while creating/inviting an entity it throws/returns an error mentioning `Role creation exception`. This could be because `admin-api,` the client which is configured for the registry does not have the required roles. The below steps need to be configured for the same.

1. Goto keycloak admin console (http://keycloak-host/auth/
2. Login admin credentials
3. Goto `clients` tab in configure section on the left
4. Click on `admin-api` client and goto `Service Account Roles` tab
5. In `Client roles` drop-down, select `realm-management`
6. Make sure `manage-realm` is present `Assigned Roles` section
7. If not select `manage-realm` from `Available Roles` section and click on `Add selected`
8. Restart the registry service

## Identity Service

#### **How to fix 'The database schema is not empty. Read more about how to baseline an existing production database' error?**

Follow the steps [here](https://www.prisma.io/docs/orm/prisma-migrate/workflows/baselining) in prisma documentation to initialise the migration with empty migration. Then restart the identity service. Or you can setup from scratch, then empty your database and start identity service first, then after run other services like registry, claim-ms, keycloak etc.

## Frontend

**How to resolve CORS errors on local development environment?**

Please Refer to the link for the solution: [https://github.com/Sunbird-RC/sunbird-rc-ui/edit/main/README.md#faqs](https://github.com/Sunbird-RC/sunbird-rc-ui/edit/main/README.md#faqs)
