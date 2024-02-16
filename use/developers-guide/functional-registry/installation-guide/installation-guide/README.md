---
description: >-
  The following steps will help you to setup SunbirdRC components for a
  local/development environment.
---

# Registry CLI

## System requirements

* 4 Cores
* 8 GB RAM
* Min 100 GB disk

## Video

{% embed url="https://www.youtube.com/live/C-cmtsEuPU0?feature=share" %}

## Prerequisites

> This guide assumes a some familiarity with basic linux commands. If not, [here](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview) is a great place to start.

> Don't copy-paste the `$` signs, they indicate that what follows is a terminal command

### Terminal emulator

Linux and MacOS will have a terminal installed already. For Windows, it is recommended that you use `git-bash`, which you can install from [here](https://git-scm.com/download/win).

Type `echo Hi` in the terminal once it is installed. If installed correctly, you should see `Hi` appear when you hit enter.

### NodeJS

Installation instructions for NodeJS can be found [here](https://nodejs.org/en/download/package-manager/).

Run `node -v` in the terminal if `node` has been installed correctly:

```
$ node -v
v16.13.0
```

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

## Installing the Registry CLI

To install the official Registry CLI, run:

```
$ npm install --global registry-cli
```

> In case you encounter a permission denied/access denied error here, prefix the command with `sudo`: `sudo npm install --global registry-cli`.

To check if the Registry CLI has been installed correctly, run:

```
$ registry help
```

This should show you all the commands you can execute using the CLI.
