# Create Schemas With Custom Password

This page demonstrates how to configure the schemas to allow entities to set their own password and walks you through the create and invite Registry APIs using the example of a Student to set the password.

## Configuring A Schema

We can create a schema in the registry using the [Schema API Endpoint](../../../api-reference/schema/create-schema.md) and using JSON schema files as well

Here we are creating a `Student` schema, we would configure as following

```json
{
	"$schema": "http://json-schema.org/draft-07/schema",
	"type": "object",
	"properties": { "Student": { "$ref": "#/definitions/Student" } },
	"required": ["Student"],
	"title": "Student",
	"definitions": {
		"Student": {
			"$id": "#/properties/Student",
			"type": "object",
			"title": "Studentschema",
			"required": ["name", "phoneNumber", "email", "school"],
			"uniqueIndexFields": ["phoneNumber"],
			"properties": {
				"name": { "type": "string" },
				"phoneNumber": { "type": "string" },
				"email": { "type": "string" },
				"school": { "type": "string" },
				// this field will be considered as password
				"password": { "type": "string", "minLength": 8 }
			}
		}
	},
	"_osConfig": {
		"ownershipAttributes": [
			{
				"email": "/email",
				"mobile": "/phoneNumber",
				"userId": "/phoneNumber",
				// password ownership attribute required
				//  to map field to password
				"password": "/password"
			}
		],
		"inviteRoles": ["anonymous"]
	}
}

```

This will configure the entity to create a password while creating the entity object. Here ownership attribute password is required, Its value can be any path in the Student object we decide. If we don't set the password ownership attribute, It will take the default password [configured in the registry environment](../configuration/).

**Note: \_Password will only be used while creation of the Student object and Updating password using update entity API Endpoint is not supported.**\_

**Note: \_If the user is already created by another entity, the password will not be updated to the existing user account.**\_

## Inviting An Entity

We can create entities in the registry using the [Invite Entity API Endpoint](../../../developer-documentation/broken-reference/).

To create a `Student` entity named Pranav Agate, we would make the following API call:

**cURL**

```
curl --location \
	--request 'POST' \
	--header 'content-type: application/json' \
	--data-raw '{
		"name": "Pranav Agate",
		"phoneNumber": "1234567890",
		"email": "pranav@upps.in",
		"school": "UP Public School",
		"password": "pranav@1234"
	}' \
	'http://localhost:8081/api/v1/Student/invite'
```

**HTTPie**

```
echo '{
	"name": "Pranav Agate",
	"phoneNumber": "1234567890",
	"email": "pranav@upps.in",
	"school": "UP Public School",
	"password": "pranav@1234"
}' | http post \
	'http://localhost:8081/api/v1/Student/invite' \
	'content-type: application/json'
```

This will store the entity in the registry, create the user account in IAM (keycloak) with given password for the `Student` and return the following object:

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
	"result": { "Student": { "osid": "1-9d6099fc-2c01-4714-bceb-55ff28c482f9" } }
}
```

## **Getting the Access Token**

So to authenticate as the `Student` entity we just created, we would make the following API call:

**cURL**

```
curl --location \
	--request POST \
	--header 'content-type: application/x-www-form-urlencoded' \
	--data 'client_id=registry-frontend' \
	--data 'username=1234567890' \
	--data 'password=pranav@1234' \
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
	'password=pranav@1234' \
	'grant_type=password'
```

> Here, `registry-frontend` is the pre-configured client we use to make requests to keycloak and `pranav@1234` is the password for the `Student` entity, we created.

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

## Retrieving An Entity

We can retrieve entities in the registry using the [Retrieve Entity API Endpoint](../../../developer-documentation/broken-reference/).

So to retrieve the entity we created earlier, we would make the following request:

**cURL**

```
curl --location \
	--request GET \
	--header 'content-type: application/json' \
	--header 'authorization: bearer {access-token}' \
	'http://localhost:8081/api/v1/Student'
```

**HTTPie**

```
http get \
	'http://localhost:8081/api/v1/Student' \
	'authorization: bearer {access-token}'
```

> Replace the `{id}` above with the entity's `osid` you saved from the create entity request. Replace the `{access-token}` with the `Student` entity's access token from the consent/authentication step.

This will return the entity's JSON representation as follows:

```json
{
	"name": "Pranav Agate",
	"phoneNumber": "1234567890",
	"email": "pranav@upps.in",
	"school": "UP Public School",
	"osid": "xxxxxx",
	"osOwner": ["xxxxxx"],
	"_osState/school": "DRAFT"
}
```

Here password won't be returned. Password is used only in the creation of the Student entity in Keycloak and not stored directly in the database.
