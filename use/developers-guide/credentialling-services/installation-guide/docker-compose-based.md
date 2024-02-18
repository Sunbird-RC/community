# Docker compose based

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

* Download the latest docker-compose file[https://github.com/Sunbird-RC/devops/blob/main/deploy-as-code/docker/v2/credentialling/docker-compose.yml](https://github.com/Sunbird-RC/devops/blob/main/deploy-as-code/docker/v2/credentialling/docker-compose.yml). Modify the values inside the compose file based on the requirements. More details on the configurations can be found [here](../configurations.md)
* Download or create a `.env` file using the example file available [here](https://github.com/Sunbird-RC/devops/blob/main/deploy-as-code/docker/v2/credentialling/.env.example).
* Start all the services, `docker-compose up -d`
* Check if all the services are started using `docker-compose ps`
* Access the credentialling services swagger: `<base-url>/docs`
