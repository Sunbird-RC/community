---
description: >-
  This page provides description on how can `registry service` be run in a local
  environment
---

# Run for development

Use the project [sunbird-rc-core](https://github.com/Sunbird-RC/sunbird-rc-core), java folder in it contains all the modules of registry as well as claims service

### Java Version

JDK version 1.8

### Install dependencies and Run Application

1. Change directory to java
2. Run `./mvnw clean install`&#x20;
3. Setup the Database and other [feature dependent services](run-for-development.md#configuring-feature-services)
4. Use the required environment variables
   1.  These are the minimum required envs to run the application -

       connectionInfo\_uri, connectionInfo\_username, connectionInfo\_password, authentication\_enabled
   2. Environment variables starting with connectionInfo\_ are for the database. Check the [configurations](../configuration/#registry-configurations) for more detailed environment variables description
   3. Check the registry service in docker-compose.yml file at the project folder for example and references
   4. Rest of the environment variables depends on the features enabled
5. Run the spring boot application `SunbirdRCApplication` in package `dev.sunbirdrc.registry.app` , Use the IDE to configure and run or follow below steps -
   1. cd to registry package
   2. Export the environment variables
   3. Run `../mvnw spring-boot:run`

### Configuring Feature Services

The feature services can be run anywhere and should be available to where we run. For local development, these can be run using docker-compose.yml file present in the project directory.

Check the [configurations](../configuration/#registry-configurations) to configure environment variables for specific services. below are most of the services used and how they can be enabled and used -

1. Encryption Service - encryption\_enabled is true
2. Id Gen Service - idgen\_enabled is true
3. All Credentials services -
   1. When signature\_enabled is set to true
   2. Make sure to set did\_enabled as true
   3. certificate\_enabled should be set to true to generate pdfs
   4. signature\_provider is set to dev.sunbirdrc.registry.service.impl.SignatureV2ServiceImpl
   5. check the other envs starting with signature\_v2\_ and did\_ in the configuration are set by default in the docker-compose.yml file
4. Old Credentialing services -
   1. When signature is enabled and&#x20;
   2. signature\_provider is set to dev.sunbirdrc.registry.service.impl.SignatureV2ServiceImpl
5. Keycloak Service -
   1. When authentication\_enabled is true and using [#create-login-for-registry](../schema-setup/schema-configuration.md#create-login-for-registry "mention") in the schemas
   2. check environment variables starts with sunbird\_sso\_ sunbird\_keycloak and auth2\_resource
6. ElasticSearch Service -
   1. ElasticSearch version supported is v6
   2. Required when search\_providerName is set to dev.sunbirdrc.registry.service.ElasticSearchService
   3. Use environment variables starting with elastic\_search\_ for custom configurations
7. Claims Service -
   1. When claims\_enabled is set to true
8. File Storage Service-
   1. When filestorage\_enabled is set as true
   2. Check config starting with filestorage\_ to configure
   3. This uses minio, so it can be a cloud storage like s3 as well or a local minio service
9. Kafka & Zookeeper Service -
   1. When using asynchronous features
   2. async\_enabled for creating entities asynchronously
   3. Or when event\_enabled for metrics service
   4. Or when notification\_enabled and notification\_async\_enabled are set to true
10. Metrics Service -
    1. When event\_enabled is set to true, check envs start with event\_ to configure
11. Notification Service -
    1. When notification\_enabled is set to true, check envs start with notification\_ to configure
12. Redis Service -
    1. When using manager\_type as DistributedDefinitionsManager
    2. Check redis\_ envs to configure

#### Setting Keycloak

1. Let's say we use http://localhost:8080/auth to generate the token
2. Then use the above url to set as frontend url in keycloak and restart the registry service
3. Steps to set frontend url
   1. Login to keycloak ui
   2. Select realm sunbird-rc
   3. Select general settings tab
   4. Check for the field frontend url
   5. Set as http://localhost:8080/auth
   6. It should be changed when deploying as per DNS used to generate a token

### Running Other Services for development

Check for the dependent services like database for encryption and id gen services and run them.

### Troubleshooting

#### Prisma Migration Issue

It occurs due to prisma migration tables are not set and the database is not empty.

1. If it is possible to cleanup the database, then cleanup the database and then setup the identity service first.
2. To fix in the production or where cleanup of the database is not possible, use this link [https://www.prisma.io/docs/orm/prisma-migrate/workflows/baselining](https://www.prisma.io/docs/orm/prisma-migrate/workflows/baselining) to baseline the database and restart the identity, credential-schema, credential services.



