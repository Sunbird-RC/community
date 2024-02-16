# High-Level architecture

<figure><img src="../../../.gitbook/assets/functional registry.drawio (1).png" alt=""><figcaption><p>High Level Architecture Diagram</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Artboard 9 (1).png" alt=""><figcaption><p>Key Services</p></figcaption></figure>

The functionalities and purpose of various microservices built as part of Sunbird-RC are explained below.

#### Registry (Core)

This is the core service which enables the major functionalities of SunbirdRC. It exposes API that can be used to configure schemas and manage entities and workflows. It can also process creating entities synchronously or asynchronously. The registry is enabled to support various database providers, which are:

1. Graph database (Neo4J)
2. Relational databases (Postgresql, HSQLDB, H2, MariaDB, MySQL, MSSQLServer)
3. NoSQL databases (Cassandra)

The registry generates REST APIs for the schemas that are created dynamically. It also applies authentication and authorization on schema APIs based on the schema configuration. The registry service also provides discovery API, which can be used to search public data of a particular schema. Elasticsearch needs to be configured to the registry to enable the discovery of schemas.

#### Claim ms

This service needs to be enabled if attestation/workflows are required. This service is responsible for handling all the claims of an attestation.

#### Notification ms

This service is required to send SMS or emails to the users. This service is used by the registry to send invite notifications to users. It is also used by keycloak to send OTP messages. This service can be configured with 3rd party plugins to send notifications.

#### Metrics service

This service is used to handle all the events emitted by registry through kafka. The service stores this events in clickhouse. However the service can be used to connect different databases. Service also exposes an API which returns all the events emitted

#### Clickhouse

Clickhouse is a open source database. We are using clickhouse to store all the events that are emitted in this clickhouse

#### DB

The registry requires a main DB that is used as the main store for storing all data. The following DB providers can be used:

1. Graph database (Neo4J)
2. Relational databases (Postgresql, HSQLDB, H2, MariaDB, MySQL, MSSQLServer)
3. NoSQL databases (Cassandra)

#### Elastic Search

Elastic search is used to store all public data and it enables the discovery of data.

#### Keycloak

Keycloak is used to enable authentication and authorization of the users on using the APIs. This can be replaced by any OpenID Connect Identity Provider and an OAuth2 compliant resource server.

#### Kafka

Kafka service is required to create entities asynchronously. If the system needs to handle high load and high availability the entity creation(with generating credentials) can be processed asynchronously using Kafka.

**Encryption**

Encryption service can be used to store private fields configured in a schema in encrypted form in db, which can only be decrypted by the encryption service. It requires to be enabled through appropriate environment variables.

**ID Gen**

ID Gen service can be used to generate IDs with given format for any fields in the schema. It requires format configurations to be provided in schema configurations. and enabled through environment variables.

{% embed url="https://youtu.be/mZjYgdxu0gU" %}
Video recorded on Dec 22. The video is recorded for v0.0.13
{% endembed %}
