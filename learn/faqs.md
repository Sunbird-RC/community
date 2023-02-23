# FAQs

## Registry

#### **Can an existing database, which gets routinely updated tho’ Business processes of department, be used as a source to create and dynamically update a sunbird powered registry?**

Yes, we can treat the Sunbird-registry as a master registry (a layer above the source databases), which is updateable (hence, “live” at all times) by any approved changes in the source databases. However, what constitutes an “update” and how should a “change in the source database” result in an “update” in the master-registry is determined by the business processes & rules established by the program. Technically, this is feasible.

#### Can excel sheet or other dBs be used as an one time source for creating a registry?

Yes, this can be done through scripts that allow bulk import of the source data into the registry.

#### Can we configure rules based validation on data which comes into the registry either through API or data being input through a form?

Yes, since SunbirdRC supports defining a registry schema using a JSONSchema format, the properties can be defined with validations like mentioned here: https://json-schema.org/draft/2019-09/json-schema-validation.html#:\~:text=JSON%20Schema%20validation%20asserts%20constraints,descriptive%20metadata%20and%20usage%20hints.

#### Is there a facility to easily create a form for inputting data into registry followed by approval process?&#x20;

Yes, the SunbirdRC UI supports the generation of dynamic forms, based on the schema created; as well as support creating “approval” workflows to aid the attestation & subsequent persistence of the “data” into the registry.

#### Can schema be re-defined (i.e., adding some extra data fields or removing certain fields)? If Yes, how versioning is managed and what happens to existing requests with old schema.&#x20;

No, it doesn't support versioning now. This needs to be handled manually outside Sunbird RC 2.&#x20;

#### Can validations be defined at field level, at the database level and also at the user interface level?&#x20;

Yes, field and user interface level validations can be defined

#### Does bulk upload of facilities create schema automatically, if yes all that one can do while creating schema be achieved through bulk upload also?&#x20;

The schema needs to be created first, before seeding the data to the registry

#### **The registry is throwing unauthorized issues (401) on retrieving entity. The registry throws "getaddrinfo ENOTFOUND keycloak" error.**

In dev/local setup, it is required that keycloak hostname is added to '/etc/hosts' file.&#x20;

```bash
vi /etc/hosts
# add the below content to the above file.
...
127.0.0.1 keycloak
...
```

Use \`[http://keycloak:8080/auth/realms/sunbird-rc/protocol/openid-connect/token](http://keycloak:8080/auth/realms/sunbird-rc/protocol/openid-connect/token)\` url to fetch the token

## Ownership

#### Can citizens be allowed to login into the registry to view and suggest updation to his registry entry?&#x20;

Yes, Sunbird-RC supports “user login” and also ownership management for a schema. Hence if the registry needs to have “citizen” access; it can be enabled. Just a point to reiterate, the purpose of a registry can be non-individual centric as well (in which case, the authorized “user”, can be institutional staff roles as well).

**Can we we use  single sign on? or use any other authentication?**

Currently, Sunbird works on Keycloak OAuth. However, we can write a custom service to authenticate the user, with other authentication methods.

## Consent

#### Is consent given individually or can be given to a group of individuals or entities**?**

Currently, consent is given by an individual to the entities that he/she has ownership of (i.e. to the entities which were created by him/her). Consent can be given to one or more attributes that are part of the schema.

#### Can consent be revoked or made time bound or limited to some number of times?

Currently, consent can only be revoked manually. For time/count-based revocation of consent, we need to build a custom service to handle it.

## Attestation

#### Can we have automatic verification and attestation through existing databases?

Yes, attestation can be done with an external system. We need to write the functionality to enable data exchange between these two systems.

## **Postgres**

#### For what all purpose Postgres is used**?**

We have three services that uses Postgres**.**

1. Keycloak: Data related to user management, client management, realm management, consent etc will be stored
2. Registry core: All data created under the schemas will be stored.&#x20;
3. Claims: All data related to attestation flows(claims) will be stored.&#x20;

Sunbird RC can work on the below DBs:&#x20;

1. Graph database (Neo4J)&#x20;
2. Relational databases (Postgresql, HSQLDB, H2, MariaDB, MySQL, MSSQLServer)&#x20;
3. NoSQL databases (Cassandra)
