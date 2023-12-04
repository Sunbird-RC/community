# Tech Stack and Requirements

## Architecture Diagram

<figure><img src="../../.gitbook/assets/Artboard 5 (1).png" alt=""><figcaption></figcaption></figure>

## Tech Stack

| Software/Language | Recommended | Minimum Version |
| ----------------- | ----------- | --------------- |
| Java              | 1.8         | 1.8             |
| Node JS           | v18.12.1    | v18.12.1        |
| Go Lang           | 1.15        | 1.15            |
| Linux / Docker    | any         | any             |
| Postgre sql       | latest      | v8              |
| Redis             | latest      | 4               |
| ElasticSearch     | 7.17.13     | 7.10.1          |
| Keycloak          | 14.0.0      | 14.0.0          |
| Minio             | latest      |                 |
| Kafka (Zookeeper) | latest      |                 |
| nginx             | latest      |                 |
| Angular           | 8           |                 |
| Kubernetes        | latest      |                 |
| Helm              | latest      |                 |
| Docker Compose    | latest      |                 |

## Stock Frontend Interface

Angular 8

## Deployment

On local machines, Docker is recommended.

For development, a single VM with docker will work.

The steps to setup the development environment is available here.

{% content-ref url="../../use/getting-started/installation-guide/" %}
[installation-guide](../../use/getting-started/installation-guide/)
{% endcontent-ref %}

For production, Kubernetes is recommended.

## Typical Requirement for production

* 1 node small (backed by CDN) 2 core 4 gb minimum (UI)
* 3 node kubernetes cluster with 4 core 8 gb RAM 200 GB storage
* 2 node for postgresql 4 core 8gb RAM 500 GB storage
* 1 node for elastic-search 4 core 8gb RAM 200 GB storage (if needed)
* Loadbalancer based on cloud provider
