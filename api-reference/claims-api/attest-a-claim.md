# Attest A Claim

{% swagger method="post" path="/api/v1/{entity-type}/claims/{claimId}/attest" baseUrl=" " summary="Attest a particular claim" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="entity-type" required="true" %}
Type of entity to which claim was sent
{% endswagger-parameter %}

{% swagger-parameter in="path" name="claimId" required="true" %}
ID of claim that is to be attested
{% endswagger-parameter %}

{% swagger-parameter in="body" name="{ "action": "action-type"}" required="true" %}
action type can be either 

`GRANT_CLAIM`

 or 

`REJECT_CLAIM`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" required="true" %}
Set to 

`Bearer {access-token}`

. 

`access-token`

 should have a role with name as entity-type 
{% endswagger-parameter %}

{% swagger-parameter in="header" name="content-type" required="true" %}
Set to 

`application/json`
{% endswagger-parameter %}
{% endswagger %}

### Usage

#### cURL

```shell
curl --location --request POST '{registry-url}/api/v1/{entity-type}/claims/a52708f8-c06f-4df7-a078-d567ec769637/attest' \
--header 'Authorization: Bearer {access-token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "action":"GRANT_CLAIM"
}'
```

#### HTTPie

```shell
printf '{
    "action":"GRANT_CLAIM"
}'| http POST '{registry-url}/api/v1/{entity-type}/claims/a52708f8-c06f-4df7-a078-d567ec769637/attest' \
 Authorization:'Bearer {access-token}' \
 Content-Type:'application/json'
```

`{registry-url}` is usually http://localhost:{port}. The port can be found under the `registry` section in the `docker-compose.yml` file and is usually `8081`
