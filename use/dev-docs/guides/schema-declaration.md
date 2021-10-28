# Creating Your Own Schemas

Each registry can store data as entities. An entity is defined by its schema.
This guide demonstrates how to create a schema to define your own entity.

The following is an example schema for a student entity. The comments should
guide you as to which fields are required, and what fields should be adjusted as
per your needs.

```jsonc
// -- file: student.json
// -- note: real schema files do not allow inline comments, remove them all incase you are copying and pasting this example.

{
	// This must be at the top of all schemas. See http://json-schema.org/understanding-json-schema/reference/schema.html#schema
	"$schema": "http://json-schema.org/draft-07/schema",
	// This is a schema object...
	"type": "object",
	// ...that declares a Student entity
	"properties": { "Student": { "$ref": "#/definitions/Student" } },
	// The actual definition of the Student entity
	"definitions": {
		// You can declare multiple entities in one schema file, just make sure you
		// add the entity name in the `properties` field above
		"Student": {
			// The path to the declaration in the schema entity
			"$id": "#/properties/Student",
			// A Student is an object...
			"type": "object",
			// ...with the following required fields
			"required": ["name", "phoneNumber", "email", "school"],
			// The `phoneNumber` field must be unique across all Student entities
			"uniqueIndexFields": ["phoneNumber"],
			// The fields a Student entity can have
			"properties": {
				// Full name (string)
				"name": { "type": "string" },
				// Phone number (string)
				"phoneNumber": { "type": "string" },
				// Email (string)
				"email": { "type": "string" },
				// What school they are going to (string)
				"school": { "type": "string" }
			}
		}
	},
	// Sunbird-RC specific configuration
	"_osConfig": {
		// The following field is only needed if the entity is a human that can
		// login, grant consent, and manually attest claims. It will (usually) not
		// be needed for something like a book or toy entity.
		// The following fields are used to create a corresponding user in Keycloak,
		// the authentication service used by Sunbird RC.
		"ownershipAttributes": [
			{
				// The path to the field to consider the email of the entity
				"email": "/email",
				// The path to the field to consider the phone number of the entity
				"mobile": "/phoneNumber",
				// The path to the field to consider as unique ID of the entity
				"userId": "/phoneNumber"
			}
		],
		// The following fields are only needed if you are using the claim-attest
		// flow for certain fields of the entity
		// Which fields' values must be attested before they can be considered valid
		"attestationAttributes": ["school"],
		"attestationPolicies": [
			// For each field mentioned in the `attestationAttributes` field, add
			// an object like so:
			{
				// The name of the field
				"property": "school",
				// The path to the field (dot-separate fields if the field is nested, $
				// is the root of the entity)
				"paths": ["$.school"],
				// Set the attestation type to `MANUAL` if another entity needs to login
				// and verify the claim
				"type": "MANUAL",
				// What type of entity should be able to attest the claim
				"attestorEntity": "Teacher",
				// The condition for a certain entity to be able to attest the claim. In
				// this case, the attestor Teacher entity must be in the same school as
				// the Student entity to attest the claim
				"conditions": "(ATTESTOR#$.school#.contains(REQUESTER#$.school#))"
			}
		]
	}
}
```
