# Technical Requirements

## Architecture Diagram

![](<../.gitbook/assets/Screenshot 2022-03-15 at 8.19.08 AM.png>)

## Requirements for Service / API

| Software       | Recommended | Minimum Version |
| -------------- | ----------- | --------------- |
| Java           | 1.8         | 1.8             |
| Linux / Docker | any         | any             |
| Postgre sql    | v12         | v8              |
| Redis          | 6           | 4               |

## Other tooling used

Keycloak for Authentication

Elastic Search for Discovery

## Stock Frontend Interface

Angular 8

## Deployment

On local machines, Docker is recommended.

For development, a single VM with docker will work.

For production, Kubernetes is recommended.

## Typical Requirement for production

* 1 node small (backed by CDN) 2 core 4 gb minimum (UI)
* 3 node kubernetes cluster with 4 core 8 gb RAM 200 GB storage
* 2 node for postgresql 4 core 8gb RAM 500 GB storage
* 1 node for elastic-search 4 core 8gb RAM 200 GB storage (if needed)
* Loadbalancer based on cloud provider
