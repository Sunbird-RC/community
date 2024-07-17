---
description: >-
  This page is dedicated for the details of how to use hashicorp vault in
  sunbird rc using docker and what all the things related to it should we
  remember.
---

# Working with the Vault

## Usage of the Vault

It is used to **store and retrieve** private keys which are used to sign the verifiable credentials.

## Dependent service

The below service is dependent on the vault -

* Identity Service - [identity-service-apis.md](../../../api-reference/credentialling-apis/identity-service-apis.md "mention")

## Setting up the Vault Manually

There are some steps followed to setup the vault

### Initialising the vault

* It is a one time process
* It can be done using cmd `vault operator init` inside vault container
* The response has unseal keys in it and a root token which needs to be stored safely

### Unsealing the vault

* Vault should be unsealed whenever it gets restarted or recreated while having the same volume or data
* Use cmd `vault operator unseal` to unseal the vault
* It should ask for unseal key
* The key here should be from generated in the [#initialising-the-vault](working-with-the-vault.md#initialising-the-vault "mention") step
* This unseal command should be run with 3 different keys to unseal
* After unsealing the vault, the container should show `healthy` status

### Enable a key-value path kv

* To enable a key value path kv of type kv-v2 , follow below steps
* Login to the vault using the root token generated in the [#initialising-the-vault](working-with-the-vault.md#initialising-the-vault "mention")
* cmd to login `vault login` inside the container vault, then run&#x20;
* `vault secrets enable -path=kv kv-v2`&#x20;

### Use the root token for identity service to work

Provide the value token to identity service environment variable `VAULT_TOKEN`

## Setting up the vault using the script

All of the above steps are created into a bash script [here](https://github.com/Sunbird-RC/sunbird-rc-core/blob/main/setup\_vault.sh).  Run below command to setup the vault OR can check if you require docker-compose specific commands -

```bash
bash setup_vault.sh docker-compose.yml vault
```

If you are using [sunbird-rc-core](https://github.com/Sunbird-RC/sunbird-rc-core) repository, then you can also use `make compose-init` to run the above cmd.

## Setting up the Vault in production -

Guide to setup the vault for production can be found [here](https://github.com/Sunbird-RC/devops/blob/main/deploy-as-code/helm/v2/registryAndCredentialling/README.md#chart-version--0240-and-vault-image-hashicorpvault1131)

## Troubleshoot

If the vault container is showing unhealthy -

* Check if the Vault is initialised
* Check if the Vault unsealed.
* Check if the path of type \`kv-v2\` is created at \`kv\`

If vault is showing `healthy` then there shouldn't be any issue with the vault. If identity-service is showing unhealthy or showing some error related to vault, then confirm if vault token is set[#use-the-root-token-for-identity-service-to-work](working-with-the-vault.md#use-the-root-token-for-identity-service-to-work "mention")
