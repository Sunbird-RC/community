# Creating Your Own Schemas

Each registry can store data as entities. An entity is defined by its schema. This guide demonstrates how to create a schema to define your own entity.

The following is an example schema for a student entity. The comments should guide you as to which fields are required, and what fields should be adjusted as per your needs.

> Note: Schema files cannot contain comments. The following example has been commented for your understanding only.

```json
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
        // The following property is used as an access modifier. The fields mentioned 
        // here are accessible by the owner of the entity and other users via consent.  
        // By default all fields are public.
        "privateFields": ["$.phoneNumber"],
        // The following property is used as an access modifier. The fields mentioned 
        // here are accessible only by the owner of the entity.
        // By default all fields are public.
        "internalFields": ["$.school"],
        // The following property is used to add additional audit fields to entity, to know
        // who/when created/updated the entity. 
        "systemFields": [
        "osCreatedAt",
        "osUpdatedAt",
        "osCreatedBy",
        "osUpdatedBy"
        ],
        // The following field is used to define the role who will be allowed to use the POST /api/v1/<Schema> API
        // to create the entity. The role will be validated with jwt token passed with the API.
        // If API does not require any role validation, then it can be set to anonymous
        "roles": ["admin"],
        // The following field is used to define the role who will be allowed to use the POST /api/v1/<Schema>/invite API
        // to invite the entity. The role will be validated with jwt token passed with the API.
        // If API does not require any role validation, then it can be set to anonymous
        "inviteRoles": ["admin"],
      
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
        "attestationPolicies": [
            // For each field mentioned in the `attestationAttributes` field, add
            // an object like so:
            {
              
                // The unique name for the attestation policy
                "name": "studentInstituteAttest",
                // The schema for the additional input to be captured that is not part of the schema/entity
                "additionalInput": {
                  "enrollmentNumber": {"type": "string"}
                },
                // The fields/properties to be used for attestation. The value of the below mentioned properties
                // will be extracted and sent for attestation.
                // The path to the field (dot-separate fields if the field is nested, $
                // is the root of the entity)
                "attestationProperties": {
                  "name": "$.name",
                  "educationDetails": "$.school"
                },
                // Set the attestation type to `MANUAL` if another entity needs to login
                // and verify the claim
                "type": "MANUAL",
                // What type of entity should be able to attest the claim OR the role an
                // entity should have (in Keycloak) for them to be able to attest the claim
                "attestorPlugin": "did:internal:ClaimPluginActor?entity=Teacher",
                // The condition for a certain entity to be able to attest the claim. In
                // this case, the attestor Teacher entity must be in the same school as
                // the Student entity to attest the claim
                "conditions": "(ATTESTOR#$.school#.contains(REQUESTER#$.school#))"
            }
        ],
        // This property holds the template to be used for generating the VC. It needs to be a template that follows
        // W3C standards. It can be an url or the template object can be defined inline. The signed data (VC) generated by this
        // template will be used to generate a QR Code.
        // Ex: inline: https://github.com/tejash-jl/ref-sunbirdrc-certificate/blob/main/schemas/TrainingCertificate.json#L52
        // Ex: external url: https://github.com/tejash-jl/ref-sunbirdrc-certificate/blob/main/schemas/SkillCertificate.json#L66
        "credentialTemplate": "",
        // This property holds the template to be used for generating visual certificate. The key can be used while 
        // downloading the certificate.
        "certificateTemplates": {
          "svg": "https://raw.githubusercontent.com/dileepbapat/ref-sunbirdrc-certificate/main/schemas/templates/TrainingCertificate.svg"
        }
    }
}
```

To add a new entity to the registry, place the JSON file defining its schema in the schemas folder. If you have setup the registry using the Registry CLI, place the JSON file in the `config/schemas` folder. If you are running the registry from its source, place the schemas in the `java/registry/src/main/resources/public/_schemas` folder. If you are using a `docker-compose.yaml` file to start the registry, the schema folder will be mentioned in the file under the `volumes` section in the registry container's configuration.

Then restart the registry by running the following:

```
# If using a docker-compose file:
$ docker compose up --force-recreate -d
# If using the Registry CLI:
$ registry restart
```

If you restarted the containers manually (without the CLI), then wait approximately 40 seconds for the containers to start. You can view the status of the registry by running the following:

```
# If using a docker-compose file:
$ docker compose ps
# If using the Registry CLI:
$ registry status
```

> `docker compose up --force-recreate -d` is only required when a change is made to the configuration (e.g.: an environment variable is updated in the `docker-compose.yaml` file, a new entity schema file was placed in the schemas folder). If you simply want to restart the registry process running in the container, a simple `docker compose restart` or `registry restart --soft` will suffice.
