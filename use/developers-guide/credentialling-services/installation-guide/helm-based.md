# Helm based

## System requirements

Please note that the below numbers are only for reference, they will vary based on the business domain and scale.

1. Kubernetes cluster (any version above v1.26.3)
   * Master Node: 2 vCPU, 4 GB RAM, Disc Size: 50 GB, Nodes: 1
   * Worker Nodes: 4 vCPU, 8 GB RAM, Disc size: 100 GB, Nodes: 3
2. Bastion server: 2 vCPU, 4 GB RAM, Disc Size: 50GB
3. Postgres with a read replica: 4 vCPU 8 GB RAM, Disc Size: 100GB
4. ElasticSearch: 4 vCPU, 8 GB RAM, Disc Size: 100GB
5. API Gateway
6. Public domain
7. Server for Minio: 2 vCPU, 4 GB RAM, Disc Size: 100GB (Optional)
8. CDN / any other alternative for hosting UI

### Prerequisites

* Kubernetes Cluster with minimum 3 nodes
* [Helm](https://helm.sh/docs/intro/install/)
* kubectl
* Ingress ([https://kubernetes.github.io/ingress-nginx/deploy/](https://kubernetes.github.io/ingress-nginx/deploy/))
* Postgres DB (create a database for `keycloak` and `registry`)
* Domain URL (domain url mapped to Kubernetes cluster)

For more details refer to the High Level Architecture

{% content-ref url="../../../../learn/technical-overview/credentialling/high-level-architecture.md" %}
[high-level-architecture.md](../../../../learn/technical-overview/credentialling/high-level-architecture.md)
{% endcontent-ref %}

### Deployment Steps

#### Clone the Repo

````
```bash
git clone https://github.com/Sunbird-RC/deployment.git
cd helm/v2
```
````

#### Pre-check

```bash
kubectl cluster-info
kubectl get nodes
kubectl get ns
helm version
```

#### Create Namespace

```bash
kubectl create ns demo-registry
```

Feel free to use a different name for the namespace. Use the same name in the reset of the commands.

#### Vault Deployment

We use the hashicorp vault as the keystore (for more details you can refer [here](https://developer.hashicorp.com/vault)).

You can refer to this official documentation for helm-based deployment on a Kubernetes cluster. This deployment guide covers the steps required to install and configure a single HashiCorp Vault cluster.([https://developer.hashicorp.com/vault/tutorials/kubernetes/kubernetes-raft-deployment-guide](https://developer.hashicorp.com/vault/tutorials/kubernetes/kubernetes-raft-deployment-guide) )

* Please follow the steps to deploy the vault service. _And use the same namespace that you have created in the previous step_.
* Unsealing a Vault is a critical process that involves reconstructing the master key from multiple key shares. This process is designed to provide added security.

#### Here are the basic steps to unseal a HashiCorp Vault:

**Initialization (First-time setup):** If you are initializing Vault for the first time, you'll need to run the following command to initialize the Vault and obtain unseal keys and the initial root token:

```bash
vault operator init
```

The output will provide unseal keys and an initial root token(\<root\_token>). Keep this information secure. (**Make sure to have a copy of these keys or store it in a key.txt file securely**)

Please make sure to use the \<root\_token> in the values.yaml. (VAULT\_SECRET\_TOKEN : base64 format of this \<root\_token>)

**Unsealing:** When Vault starts, it is sealed, and you need to unseal it using the unseal keys. You can unseal the Vault with the following command:

```
vault operator unseal
```

You will be prompted to enter an unseal key. Repeat this process with multiple unseal keys until the required threshold is reached.

**Access Vault:** After unsealing, you can access Vault using the initial root token or other tokens with appropriate permissions.

```
vault login <root_token>
```

Replace \<root\_token> with the initial root token obtained during the initialization.

Remember that unsealing requires a specified threshold (generally 3 ) of key shares to be provided, typically set during initialization. It's a security measure to ensure that a single compromised key is not enough to unseal the Vault.

Once you have the keys secured. You have to enable a KV engine

#### Enable KV engine in Vault

Use the vault secrets enable command to enable a KV engine. In this example, we're enabling the KV version 2 engine. You can replace secret-v2 with any path you prefer.

```
vault secrets enable -path=kv kv-v2
```

This command enables the KV version 2 secrets engine at the specified path (kv). You can choose a different path based on your requirements. (But whatever name you provide here has to be used as the root\_path: [http://vault:8200/v1/kv](http://vault:8200/v1/kv) in the values.yaml)

**Create secrets**

Convert all the passwords/secrets into base64 format and update these values in `values.yaml` file **Secrets**

* VAULT\_SECRET\_TOKEN: Initial Root Token that you get when you deploy vault service (ex : hvs.\*\*\*\*\*\*\*\*\*\*\*\*\*)
* DB\_URL: Database Connection URL with username and password (Example: postgres://\{{databse\_usename\}}:\{{database\_password\}}@localhost:5432/\{{database\_name:sunbirdrc\}})

VAULT\_SECRET\_TOKEN and DB\_URL are mandatory secrets to be set. Other secrets can be set to empty

**Modify configuration values**

Configuration values related to vault address, base url and rootpath etc should be modified in values.yaml file.

**Deploy helm charts**

```
helm upgrade --install --namespace=demo-registry demo-registry helm_charts --create-namespace
```

**Output**

```
Release "demo-registry" does not exist. Installing it now.
NAME: demo-registry
LAST DEPLOYED: Thu May  4 17:02:08 2023
NAMESPACE: demo-registry
STATUS: deployed
REVISION: 1
```

**Check if all the pods are running**

```
kubectl get pods -n demo-registry
```
