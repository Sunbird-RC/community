# Working with Source Code

## Compiling The Registry

Run the `configure-dependencies.sh` script in the root of the repo as follows:

```
$ sh configure-dependencies.sh
```

Then compile the registry (this will take some time when you are running it for the first time):

```
$ cd java
$ ./mvnw clean install -DskipTests
$ cd ..
```

This should create a JAR file in the `java/registry/target` folder.

## Configuring Schemas

Create `_schemas/` folder in `java/registry/src/main/resources/public/`

Place all your schema files in the `java/registry/src/main/resources/public/_schemas/` folder.

A sample set of schemas for a simple student-teacher registry can be found [here](https://github.com/Sunbird-RC/sunbird-rc-core/tree/main/tools/cli/src/templates/config/schemas). You can learn how to write your own schemas by following [this guide](schema-setup/schema-configuration.md).

## Configure And Start Dependent Services

Run the following in terminal to download [this](https://github.com/sunbird-rc/sunbird-rc-core/blob/main/tools/cli/src/templates/examples/student-teacher/docker-compose.yaml) Docker Compose file:

```
$ curl https://raw.githubusercontent.com/sunbird-rc/sunbird-rc-core/main/tools/cli/src/templates/examples/student-teacher/docker-compose.yaml > docker-compose.yml
```

To download a minimal keycloak configuration, run the following:

```
$ curl https://raw.githubusercontent.com/sunbird-rc/sunbird-rc-core/main/tools/cli/src/templates/examples/student-teacher/imports/realm-export.json > imports/realm-export.json
```

Then start Keycloak (`kc`), Postgres (`db`), Elastic Search (`es`) and the Claims Service (`cs`) by running the following command:

```
$ docker-compose up kc db es cs
```

## Configuring The Registry

Before we can start the registry, we need to regenerate and retrieve the client secret for the `admin-api` client in Keycloak. To do that, follow these steps:

* Go to http://localhost:8080/auth/admin/master/console/#/realms.
* Login using the username `admin` and password `admin`.
* Click `Sunbird RC`.
* Click `Clients` in the panel on the left.
* Click `admin-api`.
* Click the `Credentials` tab.
* Under `Client Secret`, click `Regenerate Secret`. Copy the secret that you see in the box and paste it in the `docker-compose.yml` file in place of `INSERT_SECRET_HERE` on line 42.

## Running The Registry

Once you have completed all the above steps, run the registry using the following command:

```
$ java -jar java/registry/target/registry.jar
```
