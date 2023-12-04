---
description: >-
  This setting is required when you want to run an application in a local
  environment.
---

# Frontend - Proxy configuration

To run an application in a local environment and avoid CORS issues, you can set up a proxy configuration. CORS (Cross-Origin Resource Sharing) is a security mechanism enforced by web browsers that restricts cross-origin requests. By configuring a proxy, you can bypass CORS restrictions during development.

Here are the steps to add a proxy configuration in your project:

1. Check if there is a `proxy.conf.json` file in the root folder . If the file doesn't exist, create a new file with that name.
2. Open the `proxy.conf.json` file and add the proxy configuration for your API endpoints. The configuration should be in the following format:

```json
{
  "/registry/api/docs": {
    "target": "https://domain_name/",
    "secure": false,
    "changeOrigin": true,
    "logLevel": "debug"
  }
}
```

To run an application in a local environment and avoid CORS issues, you can set up a proxy configuration. CORS (Cross-Origin Resource Sharing) is a security mechanism enforced by web browsers that restricts cross-origin requests. By configuring a proxy, you can bypass CORS restrictions during development.

Here are the steps to add a proxy configuration in your project:

1. Check if there is a `proxy.conf.json` file in the root folder . If the file doesn't exist, create a new file with that name.
2. Open the `proxy.conf.json` file and add the proxy configuration for your API endpoints. The configuration should be in the following format:

```json
{
  "/registry/api/docs": {
    "target": "https://domain_name/",
    "secure": false,
    "changeOrigin": true,
    "logLevel": "debug"
  }
}


```

* Replace `"/registry/api/docs"` with your API path.
* Set the `"target"` value to the domain name where your APIs are hosted.
* Set `"secure"` to `true` if your domain has SSL (HTTPS), or `false` if it doesn't.
* Set `"changeOrigin"` to `true` if your backend is not hosted on the localhost server.
* `"logLevel"` is used to check whether a proxy is working or not. Proxy log levels are info (the default), debug, warn, error, and silent.

3. In the `config.json` file, update the values of the `baseUrl` and `schemaUrl` properties with `http://localhost:4200/`.

```json
{
  "environment": "development",
  "keycloak": {
    "url": "https://demo-registry.xiv.in/auth",
    "clientId": "registry-frontend",
    "realm": "sunbirdrc-frontend"
  },
  "configFolder": "/assets/config/ui-config",
  "languageFolder": "/assets/i18n",
  "title": "Application name",
  "baseUrl": "http://localhost:4200/registry/api/v1",
  "schemaUrl": "http://localhost:4200/registry/api/docs/swagger.json",
  "footerText": "Organ donation registry",
    "domainName": "https://demo-url-registry.xiv.in",
  "languages": [
    "en"
  ]
}

```

4. Run the following command to start the application:

```sh
npm start
```
