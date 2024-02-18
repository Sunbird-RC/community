# Configurations

The services can be configured by passing values to the environment.

### Identity Service

| Environment Variable | Description                                                                                                |
| -------------------- | ---------------------------------------------------------------------------------------------------------- |
| DATABASE\_URL        | JDBC URL for the database. Example: `postgres://<username>:<password>@<db_url>:<db_port>/<db_name>`        |
| VAULT\_ADDR          | Address for the Hashicorp vault                                                                            |
| VAULT\_TOKEN         | Vault API Token                                                                                            |
| VAULT\_BASE\_URL     | Vault API Endpoint: `http://<vault_address>:8200/v1`                                                       |
| VAULT\_ROOT\_PATH    | Vault Root Path for the Key-Value store:`http://<vault_address>:8200/v1/kv`                                |
| VAULT\_TIMEOUT       | API Timeout for vault in milli seconds. Example: `5000`                                                    |
| VAULT\_PROXY         | false                                                                                                      |
| SIGNING\_ALGORITHM   | Algorithm to generate Key Pair in. Example: `Ed25519Signature2020, Ed25519Signature2018, RSASignature2018` |
| JWKS\_URI            | JWKS URI of the OAuth2 Resource server for handling Bearer tokens.                                         |
| ENABLE\_AUTH         | To enable or disable authentication. Example: `false`                                                      |



### Credential Schema Service

| Environment Variable | Description                                                                                         |
| -------------------- | --------------------------------------------------------------------------------------------------- |
| DATABASE\_URL        | JDBC URL for the database. Example: `postgres://<username>:<password>@<db_url>:<db_port>/<db_name>` |
| IDENTITY\_BASE\_URL  | Address for the Identity Microservice: `http://identity-service:3332`                               |
| JWKS\_URI            | JWKS URI of the OAuth2 Resource server for handling Bearer tokens.                                  |
| ENABLE\_AUTH         | To enable or disable authentication. Example: `false`                                               |

### Credential Issuance Service

| Environment Variable | Description                                                                                         |
| -------------------- | --------------------------------------------------------------------------------------------------- |
| DATABASE\_URL        | JDBC URL for the database. Example: `postgres://<username>:<password>@<db_url>:<db_port>/<db_name>` |
| IDENTITY\_BASE\_URL  | Address for the Identity Microservice: `http://identity-service:3332`                               |
| SCHEMA\_BASE\_URL    | Address for the Credential Schema Microservice: `http://cred-schema-service:3333`                   |
| JWKS\_URI            | JWKS URI of the OAuth2 Resource server for handling Bearer tokens.                                  |
| ENABLE\_AUTH         | To enable or disable authentication. Example: `false`                                               |
