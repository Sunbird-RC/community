---
description: >-
  Run identity, credential-schema and credentials services locally for
  development
---

# Run for development

`Vault` is required to run credential services. Follow this [guide](../working-with-the-vault.md) to setup the vault.

Setup the dependent services [link](run-for-development.md#dependent-services)

### Node Js Version

v20.11.0

### Environment Variables

Check in docker-compose file - [https://github.com/Sunbird-RC/sunbird-rc-core/blob/3100322ed7ad124a6633feaec66e491a2e2abc74/docker-compose.yml#L234](https://github.com/Sunbird-RC/sunbird-rc-core/blob/3100322ed7ad124a6633feaec66e491a2e2abc74/docker-compose.yml#L234) , export these variables with values

ie. `export DATABASE_URL=postgres://postgres:postgres@db:5432/registry`

### Run the Service

Change directory to the desired services, ie. identity service

Use cmd `yarn install` to install the dependencies

Use cmd `yarn start` to start the service locally

### Dependent Services

Identity service -> Vault, Database

Credential Schema Service -> Database, Identity Service

Credentials Service -> Database, Identity Service, Credential Schema Service



<br>
