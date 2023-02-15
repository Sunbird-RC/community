# Setup A Registry Instance

#### Initialize registry

Now that you have the Registry CLI installed, we can create a new instance of a registry!

Run the following command in a directory where you wish to setup the registry. For this example, we will use `~/Registries/example/`. (`~` is short form for the user's home directory).

```bash
# Create and move into the ~/Registries/example directory
mkdir -p ~/Registries/example
cd ~/Registries/example
# Create a registry instance
registry init
```

This will present you with a set of questions to setup the registry. For the purpose of this getting started guide, you can hit enter and use the default values for the registry.

#### Registry Status

Once the registry init is successfully initialized, the status of all the services can be checked using

```bash
registry status
```

The above command should return the status of all the services, like below

```bash

| ID                     | Name                            | Status             | Port                 |
| ---------------------- | ------------------------------- | ------------------ | -------------------- |
| a559b14f7306           | certificate-api                 | running            | 8078                 |
| e2c655ee0733           | certificate-signer              | running            | 8079                 |
| 9e0bf898a9ff           | claim-ms                        | running            | 8082                 |
| 2676722038e6           | context-proxy-service           | running            | 4400                 |
| 9b7f22afac7e           | db                              | running            | 5432                 |
| 0b0225d40568           | es                              | running            | 9200, 9300           |
| 6251e52152ab           | file-storage                    | running            | 9000, 9001           |
| c009d6eb5bb9           | kafka                           | running            | 9092                 |
| 2581e17915b4           | keycloak                        | running            | 8080, 9990           |
| 2d9d6360a563           | nginx                           | running            | 80                   |
| df81b2772a03           | notification-ms                 | running            | 8765                 |
| 212be4fed2e9           | public-key-service              | running            | 3300                 |
| 13b95a5d8377           | redis                           | running            | 6379                 |
| 8b48caead08e           | registry                        | running            | 8081                 |
| a143f7d6697a           | zookeeper                       | running            | 2181                 |

```

Make sure that the status of all the services is in the `running` state. If any of the services are in `exited` status you can restart only that particular service.&#x20;

```bash
docker-compose restart <service-name>
```

To restart the registry use the below command

```sh
SCHEMA_DIR=config/schemas docker-compos up -d --force-recreate --no-deps registry
```

