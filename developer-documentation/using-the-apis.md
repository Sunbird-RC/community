# Using The APIs

The following guide walks you through the different Registry APIs using the example of a student-teacher registry.

## Inviting An Entity

We can create entities in the registry using the [Invite Entity API Endpoint](broken-reference/).

To create a `Teacher` entity named Pranav Agate who teaches Math at UP Public School, we would make the following API call:

**cURL**

```
curl --location \
	--request 'POST' \
	--header 'content-type: application/json' \
	--data-raw '{
		"name": "Pranav Agate",
		"phoneNumber": "1234567890",
		"email": "pranav@upps.in",
		"subject": "Math",
		"school": "UP Public School"
	}' \
	'http://localhost:8081/api/v1/Teacher/invite'
```

**HTTPie**

```
echo '{
	"name": "Pranav Agate",
	"phoneNumber": "1234567890",
	"email": "pranav@upps.in",
	"subject": "Math",
	"school": "UP Public School"
}' | http post \
	'http://localhost:8081/api/v1/Teacher/invite' \
	'content-type: application/json'
```

This will store the entity in the registry and return the following object:

```json
{
	"id": "open-saber.registry.invite",
	"ver": "1.0",
	"ets": 1634198998956,
	"params": {
		"resmsgid": "",
		"msgid": "3ee6a76f-d6c8-4262-a7ee-ddbe66fcb127",
		"err": "",
		"status": "SUCCESSFUL",
		"errmsg": ""
	},
	"responseCode": "OK",
	"result": { "Teacher": { "osid": "1-9d6099fc-2c01-4714-bceb-55ff28c482f9" } }
}
```

Important variables in the response body:

| Field                       | In   | Type     | Description                                                                                    |
| --------------------------- | ---- | -------- | ---------------------------------------------------------------------------------------------- |
| `result.{entity-type}.osid` | body | `string` | The ID of the create entity in the registry, used for retrieval and modification of the entity |

## Requesting Access To An Entity's Data

A client application can request access to an entity's data through an OAuth 2.0 flow by creating a scope in the registry that maps (via the `User Property` mapper) the scope to give the client permission to access the data.

To go through the consent flow, we must first decide which scope to request. In this case, we will be requesting the `openid` scope to get access to all the public fields of the entity. Then, we must construct a URL to request the entity to grant us access to their data as follows:

> The following example has been indented and split into multiple lines for readability only.

```
# Keycloak's consent endpoint
http://localhost:8080/auth/realms/sunbird-rc/protocol/openid-connect/auth?
 scope=openid& # The space separated list of scopes we want the entity to grant us access to
 response_type=code& # Return an authorization code once the entity grants access
 redirect_uri=*& # Send the code to this URL
 client_id=registry-frontend # The client requesting access to the entity's data
```

Here is what the URL looks like when it's url-encoded:

`http://localhost:8080/auth/realms/sunbird-rc/protocol/openid-connect/auth?scope=openid&response_type=code&redirect_uri=*&client_id=registry-frontend`

To go through the consent flow, click on the URL and login as an entity. In this case, we can login as the `Teacher` entity we created in the [Inviting An Entity section](using-the-apis.md#inviting-an-entity) - enter `1234567890` as the username and `test` as the password.

> Here, `registry-frontend` is the preconfigured client we use to make requests to keycloak and `test` is the default password for all entities.

Once you have authenticated yourself as the `Teacher`, you will see a consent screen, asking you to grant access to `Registry Frontend`. Click `YES` to grant access to the client and continue with the consent flow.

Once you click `YES`, it will redirect you to `http://localhost:8080/auth/*`. You will see an error page, as we have not setup a frontend application to parse the response and request an access token automatically. For this example (and to gain a better understanding of how the consent flow works), we will parse keycloak's response manually.

Notice that the URL query parameters contain two variables: `session_state` and `code`. The `code` parameter is of most importance here - it is a one-time code that will allow us to retrieve an access token with access to the entity's data. Copy the value of the `code` parameter (everything after `code=` in the URL). To retrieve an access token, we make the following request:

**cURL**

```
curl --location \
	--request POST \
	--header 'content-type: application/x-www-form-urlencoded' \
	--data 'client_id=registry-frontend' \
	--data 'code={code}' \
	--data 'redirect_uri=*' \
	--data 'grant_type=authorization_code' \
	'http://localhost:8080/auth/realms/sunbird-rc/protocol/openid-connect/token'
```

**HTTPie**

```
http --form post \
	'http://localhost:8080/auth/realms/sunbird-rc/protocol/openid-connect/token' \
	'content-type: application/x-www-form-urlencoded' \
	'client_id=registry-frontend' \
	'code={code}' \
	'redirect_uri=*' \
	'grant_type=authorization_code'
```

> If you get a `invalid_grant: Code not valid` error, just go through the consent flow again. The `code` expires quickly, so try to make the request for the access token as soon as you get redirected to the redirect URL!

This API call should return a JSON object as follows:

```json
{
	"access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lk...2cSSaBKuB58I2OYDGw",
	"expires_in": 300,
	"not-before-policy": 0,
	"refresh_expires_in": 1800,
	"refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lk...9HulwVv12bBDUdU_nidZXo",
	"scope": "openid email profile",
	"session_state": "300f8a46-e430-4fd6-92aa-a2d337d7343e",
	"token_type": "Bearer"
}
```

Important variables in the response body:

| Field          | In   | Type     | Description                                                                    |
| -------------- | ---- | -------- | ------------------------------------------------------------------------------ |
| `access_token` | body | `string` | Access token used to retrieve/update entity                                    |
| `expires_in`   | body | `number` | Number of seconds before the access token will be declared invalid             |
| `token_type`   | body | `string` | Should be `Bearer`, else we have gotten the wrong token                        |
| `scope`        | body | `string` | This should contain `openid`, and this means we successfully got user consent! |

Once we have the access token, we can start retrieving and modifying entity data.

## Authenticating As An Entity

We can authenticate as entities using the [Authenticate As Entity API Endpoint](../api-reference/registry/authenticating-as-an-entity.md). **This step is only required if consent is turned off for the frontend client.**

So to authenticate as the `Teacher` entity we just created, we would make the following API call:

**cURL**

```
curl --location \
	--request POST \
	--header 'content-type: application/x-www-form-urlencoded' \
	--data 'client_id=registry-frontend' \
	--data 'username=1234567890' \
	--data 'password=test' \
	--data 'grant_type=password' \
	'http://localhost:8080/auth/realms/sunbird-rc/protocol/openid-connect/token'
```

**HTTPie**

```
http --form post \
	'http://localhost:8080/auth/realms/sunbird-rc/protocol/openid-connect/token' \
	'content-type: application/x-www-form-urlencoded' \
	'client_id=registry-frontend' \
	'username=1234567890' \
	'password=test' \
	'grant_type=password'
```

> Here, `registry-frontend` is the preconfigured client we use to make requests to keycloak and `test` is the default password for all entities.

This API call should return a JSON object as follows:

```json
{
	"access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lk...2cSSaBKuB58I2OYDGw",
	"expires_in": 300,
	"not-before-policy": 0,
	"refresh_expires_in": 1800,
	"refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lk...9HulwVv12bBDUdU_nidZXo",
	"scope": "email profile",
	"session_state": "300f8a46-e430-4fd6-92aa-a2d337d7343e",
	"token_type": "Bearer"
}
```

Important variables in the response body:

| Field          | In   | Type     | Description                                                        |
| -------------- | ---- | -------- | ------------------------------------------------------------------ |
| `access_token` | body | `string` | Access token used to retrieve/update entity                        |
| `expires_in`   | body | `number` | Number of seconds before the access token will be declared invalid |
| `token_type`   | body | `string` | Should be `Bearer`, else we have gotten the wrong token            |
| `scope`        | body | `string` | Using this token, what information we can access about the entity  |

## Retrieving An Entity

We can retrieve entities in the registry using the [Retrieve Entity API Endpoint](broken-reference/).

So to retrieve the entity we created earlier, we would make the following request:

**cURL**

```
curl --location \
	--request GET \
	--header 'content-type: application/json' \
	--header 'authorization: bearer {access-token}' \
	'http://localhost:8081/api/v1/Teacher/{id}'
```

**HTTPie**

```
http get \
	'http://localhost:8081/api/v1/Teacher/{id}' \
	'authorization: bearer {access-token}'
```

> Replace the `{id}` above with the entity's `osid` you saved from the create entity request. Replace the `{access-token}` with the `Teacher` entity's access token from the consent/authentication step.

This will return the entity's JSON representation as follows:

```json
{
	"phoneNumber": "1234567890",
	"school": "UP Public School",
	"subject": "Math",
	"name": "Pranav Agate",
	"osid": "{id}",
	"osOwner": ["{owner-id}"],
	"_osState/school": "DRAFT"
}
```

Important variables in the response body:

| Field              | In   | Type     | Description                                                                                                                                                                                                                            |
| ------------------ | ---- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `osOwner`          | body | `string` | User ID of the entity in Keycloak.                                                                                                                                                                                                     |
| `_osState/{field}` | body | `string` | State of an attestable field. Can be `DRAFT` (when it has not been sent for attestation), `ATTESTATION_REQUESTED` (when sent for attestation), `PUBLISHED` (when successfully attested) and `REJECTED` (when rejected by the attestor) |

## Updating An Entity

We can update entities in the registry using the [Update Entity API Endpoint](broken-reference/).

So to update the subject our `Teacher` entity Pranav Agate teaches to `Biology`, we would make the following API call:

**cURL**

```
curl --location \
	--request 'PUT' \
	--header 'content-type: application/json' \
	--header 'authorization: bearer {access-token}' \
	--data-raw '{
		"name": "Pranav Agate",
		"phoneNumber": "1234567890",
		"email": "pranav@upps.in",
		"subject": "Biology",
		"school": "UP Public School"
	}' \
	'http://localhost:8081/api/v1/Teacher/{id}'
```

**HTTPie**

```
echo '{
	"name": "Pranav Agate",
	"phoneNumber": "1234567890",
	"email": "pranav@upps.in",
	"subject": "Biology",
	"school": "UP Public School"
}' | http put \
	'http://localhost:8081/api/v1/Teacher/{id}' \
	'content-type: application/json' \
	'authorization: bearer {access-token}'
```

> Replace the `{id}` above with the entity's `osid` you saved from the create entity request. Replace the `{access-token}` with the `Teacher` entity's access token from the consent/authentication step.

> We need to send the whole entity and not just the updated fields because that is how RESTful APIs work. A PUT call should replace the existing record in the database with the new object as-is. To know more about this, take a look at the accepted answer on [this SO question](https://stackoverflow.com/questions/28459418/use-of-put-vs-patch-methods-in-rest-api-real-life-scenarios).

This will update the entity in the registry and return the following object:

```json
{
	"id": "open-saber.registry.update",
	"ver": "1.0",
	"ets": 1634371946769,
	"params": {
		"resmsgid": "",
		"msgid": "d51e6e6a-027d-4a42-84bb-2ce00e31d993",
		"err": "",
		"status": "SUCCESSFUL",
		"errmsg": ""
	},
	"responseCode": "OK"
}
```

## Deleting An Entity

We can delete entities in the registry using the [Delete Entity API Endpoint](broken-reference/).

So to delete the subject our `Teacher` entity, we would make the following API call:

**cURL**

```
curl --location \
	 --request 'DELETE' \
	 --header 'content-type: application/json' \
	 --header 'authorization: bearer {access-token}' \
	 'http://localhost:8081/api/v1/Teacher/{id}'
```

**HTTPie**

```
http DELETE \
	'http://localhost:8081/api/v1/Teacher/{id}' \
	'content-type: application/json' \
	'authorization: bearer {access-token}'
```

> Replace the `{id}` above with the entity's `osid` you saved from the create entity request. Replace the `{access-token}` with the `Teacher` entity's access token from the consent/authentication step.

This will delete the entity in the registry and return a blank HTTP 200 response.

## Making A Claim

To make a claim, we can use the [Claim API Endpoint](broken-reference/).

First, let us create a `Student` entity named Prashant Joshi who also goes to UP Public School:

**cURL**

```
curl --location \
	--request 'POST' \
	--header 'content-type: application/json' \
	--data-raw '{
		"name": "Prashant Joshi",
		"phoneNumber": "9876543210",
		"email": "prashant@upps.in",
		"school": "UP Public School"
	}' \
	'http://localhost:8081/api/v1/Student/invite'
```

**HTTPie**

```
echo '{
	"name": "Prashant Joshi",
	"phoneNumber": "9876543210",
	"email": "prashant@upps.in",
	"school": "UP Public School"
}' | http post \
	'http://localhost:8081/api/v1/Student/invite' \
	'content-type: application/json'
```

Next, we can get an access token for Prashant by either making a POST request to the authentication server (see the [Authenticating As An Entity section](using-the-apis.md#authenticating-as-an-entity) to know how to do that) OR by requesting entity consent to access their data (see the [Requesting Access To An Entity's Data section](using-the-apis.md#requesting-access-to-an-entitys-data) to know how to do that). The default registry instance setup by the CLI has consent enabled, so we will follow the consent flow to retrieve an access token.

Then, we can send the claim (that Prashant is a student at UP Public School) for attestation by making the following request:

```
curl --location \
	--request 'PUT' \
	--header 'content-type: application/json' \
	--header 'authorization: bearer {access-token}' \
	--data-raw '"UP Public School"' \
	'http://localhost:8081/api/v1/Student/{id}/school?send=true'
```

**HTTPie**

```
echo '"UP Public School"' | http put \
	'http://localhost:8081/api/v1/Student/{id}/school' \
	'content-type: application/json' \
	'authorization: bearer {access-token}' \
	'send==true'
```

> Replace the `{id}` above with the entity's `osid` you saved from the create entity request. Replace the `{access-token}` with the `Student` entity's access token from the consent/authentication step.

This will send the claim for attestation and return the following object:

```json
{
	"id": "open-saber.registry.update",
	"ver": "1.0",
	"ets": 1634371946769,
	"params": {
		"resmsgid": "",
		"msgid": "ksi38Dsl-8dIw-492j-6vlS-84KRe0Csop35",
		"err": "",
		"status": "SUCCESSFUL",
		"errmsg": ""
	},
	"responseCode": "OK"
}
```

If you retrieve the `Student` entity by following the [Retrieving An Entity section](using-the-apis.md#retrieving-an-entity), you will get the following object in response:

```json
{
	"email": "prashant@upps.in",
	"name": "Prashant Joshi",
	"phoneNumber": "9876543210",
	"school": "UP Public School",
	"osid": "{id}",
	"osOwner": ["{owner-id}"],
	"_osClaimId/school": "{claim-id}",
	"_osState/school": "ATTESTATION_REQUESTED"
}
```

Important variables in the response body:

| Field                | In   | Type     | Description                                                                                                                                                                                                                            |
| -------------------- | ---- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `osOwner`            | body | `string` | User ID of the entity in Keycloak.                                                                                                                                                                                                     |
| `_osState/{field}`   | body | `string` | State of an attestable field. Can be `DRAFT` (when it has not been sent for attestation), `ATTESTATION_REQUESTED` (when sent for attestation), `PUBLISHED` (when successfully attested) and `REJECTED` (when rejected by the attestor) |
| `_osClaimId/{field}` | body | `string` | ID of the claim made on a field, required when attesting the claim.                                                                                                                                                                    |

## Attesting/Reject A Claim

We can attest/reject an entity's claim using the [Attest Claim API Endpoint](../api/attesting-a-calim.md).

So to attest the claim we made in the previous section (that Prashant is a student at UP Public School), we make the following request:

**cURL**

```
curl --location \
	--request 'POST' \
	--header 'content-type: application/json' \
	--header 'authorization: bearer {access-token}' \
	--data-raw '{
		"action": "GRANT_CLAIM"
	}' \
	'http://localhost:8081/api/v1/Teacher/claims/{claim-id}/attest'
```

**HTTPie**

```
echo '{
	"action": "GRANT_CLAIM"
}' | http post \
	'http://localhost:8081/api/v1/Teacher/claims/{claim-id}/attest' \
	'content-type: application/json' \
	'authorization: bearer {access-token}'
```

> Replace the `{claim-id}` above with the `_osClaimId/school` you saved from the make a claim request. Replace the `{access-token}` with the `Teacher` entity's access token from the consent/authentication step. Replace `GRANT_CLAIM` with `REJECT_CLAIM` to reject the claim instead.

This will attest/reject the claim and return a blank HTTP 200 response.
