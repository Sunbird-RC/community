# Get Attestation Certificate

{% swagger method="get" path="/api/v1/{entity-type}/{entity-id}/attestation/{attestation-name}/{attestation-id}" baseUrl=" " summary="Retrieve Attestation Certificate" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-name" required="true" %}
Name of Entity for which attestation was raised
{% endswagger-parameter %}

{% swagger-parameter in="path" name="entity-id" required="true" %}
ID of entity for which attestation was raised
{% endswagger-parameter %}

{% swagger-parameter in="path" name="attestation-name" required="true" %}
Name of attestation for that entity-type
{% endswagger-parameter %}

{% swagger-parameter in="path" name="attestation-id" required="true" %}
ID of the attestation for that record of entity
{% endswagger-parameter %}

{% swagger-parameter in="header" name="accept" required="true" %}
Set to `application/pdf, application/json, text/html, image/svg+xml`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="template" %}
if `enable_external_templates` is set to true, send in a url which has html template
{% endswagger-parameter %}

{% swagger-parameter in="header" name="template-key" %}
A key pointing to html template which is present in `Schema`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="access-token" %}
Set to `Bearer {access-token}` if roles in schema is not anonymous. Else authorization can be empty
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successful retrieval of certification" %}
```markup
PDF File
```
{% endswagger-response %}
{% endswagger %}

### Usage

### cURL

```shell
curl --location --request GET '{registry-url}/api/v1/{entity-type}/{entity-id}/attestation/{attestation-name}/{attestation-id}' \
--header 'Accept: application/pdf' \
--header 'template: {url}' \
--header 'Authorization: Bearer {access-token}'
```

### HTTPie

```shell
http GET '{registry-url}/api/v1/{entity-type}/{entity-id}/attestation/{attestation-name}/{attestation-id}' \
 Accept:'application/pdf' \
 template:'{url}' \
 Authorization:'Bearer {access-token}'
```

> `{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`
