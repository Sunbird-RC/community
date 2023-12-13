# Installation

### **Pre-requisites**

It is required to have followed all the steps mentioned in the [installation guide](../../developer-documentation-1/installation-guide/)

### Configure Schemas

Once all the registry components have been successfully started, the below steps can be followed.

#### Clone the repository

{% embed url="https://github.com/Sunbird-RC/demo-education-registry" %}
Clone the above repo using https or ssh
{% endembed %}

#### Copy schema files

Copy all the schemas present in `demo-education-registry/schemas` directory to the directory where the registry CLI `<registry-cli-directory>/config/schemas` is installed.

> If any exisiting schemas are present in that directory, you can remove it or move it to a different directory.

Make sure all the schema files are present in `<registry-cli-directory>/config/schemas` directory.

```
BaseAttestationField.json
Common.json
Institute.json
Student.json
Teacher.json
```

#### Restart registry service

```bash
docker-compose up -d --force-recreate --no-deps registry
```

#### Registry status

Check the status of all the services after the restart

```bash
registry status
```

### API Documentation

The postman collection is available

{% embed url="https://github.com/Sunbird-RC/demo-education-registry/blob/main/Tech%20Webinar.postman_collection.json" %}

Jupyter Notebook

{% embed url="https://github.com/Sunbird-RC/demo-education-registry/blob/main/edu-attestation-flows.ipynb" %}
