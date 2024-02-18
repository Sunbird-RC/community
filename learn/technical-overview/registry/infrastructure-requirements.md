# Tech Stack and Requirements

## Architecture Diagram

<figure><img src="../../../.gitbook/assets/Artboard 5 (1).png" alt=""><figcaption></figcaption></figure>

## Tech Stack

<table><thead><tr><th width="284">Software/Language</th><th>Recommended</th><th>Minimum Version</th></tr></thead><tbody><tr><td>Java</td><td>1.8</td><td>1.8</td></tr><tr><td>Go Lang</td><td>1.15</td><td>1.15</td></tr><tr><td>Linux / Docker</td><td>any</td><td>any</td></tr><tr><td>Postgre sql</td><td>latest</td><td>v8</td></tr><tr><td>Redis</td><td>latest</td><td>4</td></tr><tr><td>ElasticSearch</td><td>7.17.13</td><td>7.10.1</td></tr><tr><td>Keycloak</td><td>14.0.0</td><td>14.0.0</td></tr><tr><td>Minio</td><td>latest</td><td></td></tr><tr><td>Kafka (Zookeeper)</td><td>latest</td><td></td></tr><tr><td>nginx</td><td>latest</td><td></td></tr><tr><td>Angular</td><td>8</td><td></td></tr><tr><td>Kubernetes</td><td>latest</td><td></td></tr><tr><td>Helm</td><td>latest</td><td></td></tr><tr><td>Docker Compose</td><td>latest</td><td></td></tr></tbody></table>

## Stock Frontend Interface

Angular 8

## Deployment

On local machines, Docker is recommended.

For development, a single VM with docker will work.

The steps to setup the development environment is available here.

{% content-ref url="../../../use/developers-guide/functional-registry/installation-guide/" %}
[installation-guide](../../../use/developers-guide/functional-registry/installation-guide/)
{% endcontent-ref %}

For production, Kubernetes is recommended.

## Typical Requirement for production

* 1 node small (backed by CDN) 2 core 4 gb minimum (UI)
* 3 node kubernetes cluster with 4 core 8 gb RAM 200 GB storage
* 2 node for postgresql 4 core 8gb RAM 500 GB storage
* 1 node for elastic-search 4 core 8gb RAM 200 GB storage (if needed)
* Loadbalancer based on cloud provider
