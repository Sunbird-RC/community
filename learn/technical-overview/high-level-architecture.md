# High level architecture

{% embed url="https://www.youtube.com/watch?feature=youtu.be&v=mZjYgdxu0gU" %}

<figure><img src="../../.gitbook/assets/Artboard 9 (1).png" alt=""><figcaption><p>Key Services</p></figcaption></figure>

#### High-Level Architecture Diagram

<figure><img src="../../.gitbook/assets/S-RC Arch (1).png" alt=""><figcaption><p>High Level Architecture</p></figcaption></figure>

The functionalities and purpose of various microservices built as part of Sunbird-RC are explained below.

#### Registry (Core)

This is the core service which enables the major functionalities of SunbirdRC. It exposes API that can be used to configure schemas and manage entities and workflows. It can also process creating entities synchronously or asynchronously. The registry is enabled to support various database providers, which are:

1. Graph database (Neo4J)
2. Relational databases (Postgresql, HSQLDB, H2, MariaDB, MySQL, MSSQLServer)
3. NoSQL databases (Cassandra)

The registry generates REST APIs for the schemas that are created dynamically. It also applies authentication and authorization on schema APIs based on the schema configuration. The registry service also provides discovery API, which can be used to search public data of a particular schema. Elasticsearch needs to be configured to the registry to enable the discovery of schemas.

#### Claim ms

This service needs to be enabled if attestation/workflows are required. This service is responsible for handling all the claims of an attestation.

#### Certificate Signer

This service needs to be enabled if verifiable credentials need to be generated for a schema. This service is capable of generating W3C based schemas. This service support configuring multi keys based on issuer. The service supports RSA & ED25519 based keys. The key [file](https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/services/certificate-signer/config.json) needs to be [mounted](https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/docker-compose.yml#L137) to the service.

#### Certificate API

This service needs to be enabled if a visual template for the verifiable credentials is required. This service is capable of generating a QR based template. The templates that are supported are SVG/HTML/PDF.

#### Notification ms

This service is required to send SMS or emails to the users. This service is used by the registry to send invite notifications to users. It is also used by keycloak to send OTP messages. This service can be configured with 3rd party plugins to send notifications.

#### Public key service

This service is used to expose public keys that are used to generate verifiable credentials. It has APIs that exposes all the public keys or issuer-based public key. This API will be used by the verification services to verify the issued verifiable credentials.

#### Metrics service

This service is used to handle all the events emitted by registry through kafka. The service stores this events in clickhouse. However the service can be used to connect different databases. Service also exposes an API which returns all the events emitted

#### Clickhouse

Clickhouse is a open source database. We are using clickhouse to store all the events that are emitted in this clickhouse

#### Context proxy service

This service will be used by verifying clients to proxy the context URLs that are used in the verifiable credentials. The public facing verifying clients needs to access the content of the context URLs that are present in the verifiable credentials. If the client is UI based application, the context URLs will be blocked due to CORS issues. This service can be used to overcome the CORS issue. The context URLs can be routed to this service which can proxy and return back the contents.

#### DB

The registry requires a main DB that is used as the main store for storing all data. The following DB providers can be used:

1. Graph database (Neo4J)
2. Relational databases (Postgresql, HSQLDB, H2, MariaDB, MySQL, MSSQLServer)
3. NoSQL databases (Cassandra)

#### Elastic Search

Elastic search is used to store all public data and it enables the discovery of data.

#### Keycloak

Keycloak is used to enable authentication and authorization of the users on using the APIs.

#### Nginx

The SunbirdRC package is also shipped with a custom modified Nginx image which is configured with all the reverse proxies for the services and also contains a generic verification page.

#### File Storage (minio)

S3 compatible object storage service is also shipped along with services which can be used to store files. It can be used by adopters who are running the services on private/bare metal servers.

#### Redis

The registry core service will require a Redis cache layer when the core service is scaled to multiple instances.

#### Kafka

Kafka service is required to create entities in an asynchronous fashion. If the system needs to handle high load and high availability the entity creation(with generating credentials) can be processed asynchronously using Kafka.

#### Bulk Issuance

Bulk issuance service will allow the issuers to upload their csv files and issue credentials to all those actors present in each row of a csv. The service also has the capability to return a file based reports which will download a csv file which will contain a column of errors that occurred for a that specific actors data
