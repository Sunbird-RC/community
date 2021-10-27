# Developer Setup Guide

## Prerequisites

### Terminal emulator

Linux and MacOS will have a terminal installed already. For Windows, it is
recommended that you use `git-bash`, which you can install from
[here](https://git-scm.com/download/win).

Type `echo Hi` in the terminal once it is installed. If installed correctly, you
should see `Hi` appear when you hit enter.

### Git

Installation instructions for Git can be found
[here](https://github.com/git-guides/install-git).

Run `git --version` in the terminal to check if `git` has been installed
correctly:

```sh
$ git --version
git version 2.33.0
```

### Java

Installation instructions for Java 8 can be found
[here](https://docs.oracle.comjavase/8/docs/technotes/guides/install/install_overview.html).

Run `java` in the terminal to check if `java` has been installed correctly:

```sh
$ java
Usage: java [-options] class [args...]
...
```

### NodeJS (Only needed for the Registry CLI)

Installation instructions for NodeJS can be found
[here](https://nodejs.org/en/download/package-manager/).

Run `node -v` in the terminal to check if `node` has been installed correctly:

```sh
$ node -v
v16.11.0
```

### Docker

Installation instructions for Docker can be found
[here](https://docs.docker.com/engine/install/).

Run `docker -v` in terminal to check if `docker` has been installed correctly:

```sh
$ docker -v
Docker version 20.10.9, build c2ea9bc90b
```

### Docker Compose

Installation instructions can be found
[here](https://docs.docker.com/engine/install/).

Run `docker-compose -v` in terminal to check if `docker-compose` has been
installed correctly:

```sh
$ docker-compose -v
Docker Compose version 2.0.1
```

## Downloading The Source Code

Run the following in terminal to download the registry's source code:

```sh
$ git clone https://github.com/sunbird-rc/sunbird-rc-core.git sunbird-rc/core
```

Move into the folder by typing:

```sh
$ cd sunbird-rc/core
```

## Compiling The Registry

Run the `configure-dependencies.sh` script in the root of the repo as follows:

```sh
$ sh configure-dependencies.sh
```

Then compile the registry (this will take some time when you are running it for
the first time):

```sh
$ cd java
$ ./mvnw clean install -DskipTests
$ cd ..
```

This should create a JAR file in the `java/registry/target` folder.

## Configuring Schemas

Place all your schema files in the
`java/registry/src/main/resources/public/_schemas/` folder.

A sample set of schemas for a simple teacher-student registry can be found
[here](https://github.com/gamemaker1/registry-setup-files/tree/main/schemas/).
You can learn how to write your own schemas by following
[this guide](schema-declaration.md).

## Configure And Start Dependent Services

Run the following in terminal to download
[this](https://github.com/gamemaker1/registry-setup-files/blob/main/docker-compose.yaml)
Docker Compose file:

```sh
$ curl https://raw.githubusercontent.com/gamemaker1/registry-setup-files/main/docker-compose.yaml > docker-compose.yml
```

Then edit your hosts file (`/etc/hosts` on Linux/MacOS,
`C:\Windows\System32\Drivers\etc\hosts` on Windows) and add the following line
at the end:

```
127.0.0.1 kc
```

To download a minimal keycloak configuration, run the following:

```sh
$ curl https://raw.githubusercontent.com/gamemaker1/registry-setup-files/main/imports/realm-export.json > import/realm-export.json
```

Then start Keycloak (`kc`), Postgres (`db`), Elastic Search (`es`) and the
Claims Service (`cs`) by running the following command:

```sh
$ docker-compose up kc db es cs
```

## Configuring The Registry

Before we can start the registry, we need to regenerate and retrieve the client
secret for the `admin-api` client in Keycloak. To do that, follow these steps:

- Go to http://localhost:8080.
- Login using the username `admin` and password `admin`.
- Change the current realm from `Master` by hovering over `Master` in the top
  left and clicking `Sunbird RC` when it appears.
- Click `Clients` in the panel on the left.
- Click `admin-api`.
- Click the `Credentials` tab.
- Under `Client Secret`, click `Regenerate Secret`. Copy the secret that you see
  in the box and paste it in the `docker-compose.yml` file in place of
  `INSERT_SECRET_HERE` on line 42.

## Running The Registry

Once you have completed all the above steps, run the registry using the
following command:

```sh
$ java -jar java/registry/target/registry.jar
```
