---
description: The following steps will install SunbirdRC via docker compose file
---

# Manual installation through docker-compose

## System requirements

* 4 Cores
* 8 GB RAM
* Min 100 GB disk

## Prerequisites

> This guide assumes a some familiarity with basic linux commands. If not, [here](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview) is a great place to start.

> Don't copy-paste the `$` signs, they indicate that what follows is a terminal command

### Terminal emulator

Linux and MacOS will have a terminal installed already. For Windows, it is recommended that you use `git-bash`, which you can install from [here](https://git-scm.com/download/win).

Type `echo Hi` in the terminal once it is installed. If installed correctly, you should see `Hi` appear when you hit enter.

### Docker

Installation instructions for Docker can be found [here](https://docs.docker.com/engine/install/).

Run `docker -v` in terminal to check if `docker` has been installed correctly:

```
$ docker -v
Docker version 20.10.9, build c2ea9bc90b
```

### Docker Compose

Installation instructions can be found [here](https://docs.docker.com/compose/install/).

Run `docker compose version` in the terminal to check if `docker compose` has been installed correctly:

```
$ docker compose version
Docker Compose version 2.0.1
```

## Installation

* Download the latest docker-compose file [https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/docker-compose.yml](https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/docker-compose.yml). Modify the values inside the compose file based on the requirements. More details on the configurations can be found [here](../configuration/).
* Download or create a `.env` file. [https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/.env](https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/.env)
* Add the below environment variables to `.env` file

| ENV              | Value   | Description                                                                                                                                                   |
| ---------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RELEASE\_VERSION | v0.0.14 | Use the latest release version of SunbirdRC. [https://github.com/Sunbird-RC/sunbird-rc-core/releases](https://github.com/Sunbird-RC/sunbird-rc-core/releases) |
| SCHEMA\_DIR      | schemas | Relative path to the directory where schemas are created                                                                                                      |

* Create the [schema](../schema-setup/introduction-to-schemas.md) files in the `schemas` directory
* Create a directory `imports`
* Download the [keycloak realm](https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/imports/realm-export.json) file in `imports` directory
* Download the [sample signing key](https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/imports/config.json) file in `imports` directory. Please note to update the signing keys before going to production.
*   Steps to setup keycloak:

    * Start the database container

    `docker-compose up -d db`

    * Start the keycloak container

    `docker-compose up -d keycloak`

    * Open the keycloak admin console \`[http://localhost:8080/auth/](http://localhost:8080/auth/)\`
    * Goto `Clients` -> `admin-api` -> `Credentials`
    * Click on `Regenerate Secret` and copy the new value
* Set `KEYCLOAK_SECRET` with the copied value in `.env` file
* Start all the services, `docker-compose up -d`
* Check if all the services are started using `docker-compose ps`
* Access the registry swagger json \`[http://localhost:8081/api/docs/swagger.json](http://localhost:8081/api/docs/swagger.json)\`\\
