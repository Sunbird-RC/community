# Schema Configuration

## **Schema**

All registries have attributes pertaining to the entity or the fact in question either person or things. Schema defines the structure and constraints of the entity, Sunbird RC uses standard JSON-LD based schema.

In the example given below Place is Concept that registry is storing supporting name, city, addressRegion (state) and country.

#### Example:

```
{
 "$schema": "http://json-schema.org/draft-07/schema",
 "type": "object",
 "properties": {
   "Place": {
     "$ref": "#/definitions/Place"
   }
 },
 "required": [
   "Place"
 ],
 "title": "Place",
 "definitions": {
   "Place": {
     "$id": "#/properties/Place",
     "type": "object",
     "title": "The Place Schema",
     "required": [
       "name",
       "city",
       "addressRegion",
       "country"
     ],
     "properties": {
       "name": {
         "type": "string"
       },
       "city": {
         "type": "string"
       },
       "addressLocality": {
         "type": "string"
       },
       "addressRegion": {
         "type": "string"
       },
       "country": {
         "type": "string"
       },
       "postalCode": {
         "type": "string"
       },
       "contact": {
         "type": "string"
       },
       "email": {
         "type": "string"
       }
     }
   }
 }}
```

## Configuration

In addition to JSON LD specific data types SunbirdRC supports various extension configurations under “\_osConfig”. Access, indexing, primary key etc are configurable using schema configuration.

#### Entity and Properties

#### Property data types and restrictions

Supported types:

String - Unicode text, additional regular expressions restrictions can be applied.

Enum - restricted list of values

Number - numeric data

Integer - The integer type is used for integral numbers

Object - Objects are the mapping type in JSON. They map “keys” to “values”. In JSON, the “keys” must always be strings. Each of these pairs is conventionally referred to as a “property”.

Array - Arrays are used for ordered elements. In JSON, each element in an array may be of a different type.

Boolean - The boolean type matches only two special values: true and false. Note that values that evaluate to true or false, such as 1 and 0, are not accepted by the schema.

More details are available here [Type-specific keywords](https://json-schema.org/understanding-json-schema/reference/type.html).

#### List of attributes

Collection of data values are also supported as multiple data value might represent the entity property for example subjects taught : \[“english”,”science”, “mathematics”]

### Visibility scope:

3 types of visibility on attributes.

1. Public
2. Private (privateFields)
3. Internal

Public data attributes are available in discovery by default any sort of permission / authorization is not needed by default. By default all fields are public.

Private attributes can be accessed by the owner by default, with consent 3rd party can access the data field.

Internal fields are system fields that only serve internal functionalities, these can never be accessed by any actors in the system.

### System fields:

The following property is used to add additional audit fields to entity, to know who/when created/updated the entity.

1. osCreatedAt
2. osUpdatedAt
3. osCreatedBy
4. osUpdatedBy

### Index field set

Indexes can help in faster access to information, based on usage context index fields can be configured. Example:

`"indexFields": ["phoneNumber"],`

### Primary Keys for Entities

Primary keys from domain space will help enforce uniqueness of information and make it easily accessible. “uniqueIndexFields” can be used to configure the same.

Example:

`"uniqueIndexFields": ["identityProperty"]`

### Validation Extensions

JSON LD schema allows defining basic type constraints like numeric type, text or list of items etc. Also it allows a list of possible values for given attributes (enum) and regular expressions based constraints for the value.

### Roles

#### Manage registry based on roles

Using `roles` config, one can set the authorization for crud operations on the schema

Example: let's say there are two registries Teacher and EducationCertificate, assume only Teacher can manage (add/update/delete) the EducationCertificate, then assign role "teacher" to the relevant user in a keycloak and add the "roles" : \["teacher"] in the EducationCertificate config, now only token which has the role "teacher" can manage the EducationCertificate

#### Invite based on roles

Using `inviteRoles` config, one can set the authorization for invite operation

#### Example:

**1. Only teacher can invite student**

Let's say there are two registries Teacher and Student, assume only Teacher can invite the Student, then assign role "teacher" to the relevant user in a keycloack and add the "inviteRoles" : \["teacher"] in the Student schema config, now only token which has the role "teacher" can invite the Student.

**2. Anyone can invite student**

Set "inviteRoles" : \["anonymous"] in Student schema config, now anyone can invite the Student

_**Anonymous role: Any of the above role configs can be set to anonymous to allow anonymous access to the respective operations**_

### Create login for registry

By default login is disabled for the registry, we need to set `enableLogin:true` inorder to create the user in keycloak. Login credentials for an entity can be configured through `ownershipAttributes` attributes. This attributes contains a list of objects for which a user enitity in keycloak will be created.

```
"ownershipAttributes": [
      {
        "email": "/email",
        "mobile": "/contact",
        "userId": "/contact"
        "password": "/password"
      }
    ]
```

We can have more than one owner for an entity. The respective json paths for the fields needs to be configured. All the properties are mandatory for creating an owner except password. The password is only set used for entity creation, it is not persisted in database.

### Attestation policy

List of policies can be configured for the attestation.

```json
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
```

#### Attestation Policy Attributes:

#### name

A unique name for identifying an attestation policy. This name will be used to refer an attestation policy while raising a claim.

#### attestationProperties

This field refers the schema properties which will be considered for attestation.

#### additionalInput

This denotes the additional inputs that needs to be captured outside the existing entity/schema fields for generating/processing the attestation.

#### type

This denotes if the attestation should be raised manually or automatically.

* MANUAL: In this two types of user will come into the picture, i.e Attestor and Requestor, Requestor will rise a claim, and the Attestor will approve/reject. The authorization of the attestor will be defined in the config using `conditions` attribute.

Example: Only teacher from the particular institute can attest the student,

```
Student Education
{
    "program": "HSC",
    "graduationYear": "2021",
    "marks": "100",
    "instituteOSID": "b62b3d52-cffe-428d-9dd1-61ba7b0a5882",
    "documents": [
        {
            "fileName": "e3266115-0bd0-4456-a347-96f4dc335761-blog_draft",
            "format": "file"
        },
        {
            "fileName": "e56dab1b-bd92-41bb-b9e5-e991438f27b8-NDEAR.txt",
            "format": "file"
        }
    ]
}

Teacher Experience
{
  "experience": {
      "instituteOSID": "b62b3d52-cffe-428d-9dd1-61ba7b0a5882",
      "employmentType": "Permanent",
      "start": "1999-06-16",
      "end": "2022-06-16",
      "teacherType": "Head teacher",
      "subjects": [
        "Science",
        "Mathematics"
      ],
      "grades": [
        "Class I", "Class II"
      ]
  }
}
```

For this scenario the condition would be, `conditions: "(ATTESTOR#$.experience.[*].instituteOSID#.contains(REQUESTER#$.instituteOSID#))"` The above snippet says that attestor's education institute and requestor's experience must belong to the same institute.

* AUTOMATED: If the attestation policy is `AUTOMATED` then a claim will be initiated automatically without any user intervention/API calls. When there is any addition/modification to the `attestationProperties` then the matching attestation policy will be triggered and executed.

#### attestorPlugin

This denotes who processes the claims that is been raised by the user. By default SunbirdRC will be shipped with a `ClaimPluginActor` who will process and store the respective claims in DB. If we need additional or different functionalities then we can

### Credential Template

This property holds the template to be used for generating the VC. It needs to be a template that follows W3C standards. It can be an url or the template object can be defined inline. The signed data (VC) generated by this template will be used to generate a QR Code.

* Ex: inline:

```json
{
  ...
  "credentialTemplate": {
    "@context": [
      "https://www.w3.org/2018/credentials/v1",
      "https://gist.githubusercontent.com/dileepbapat/eb932596a70f75016411cc871113a789/raw/498e5af1d94784f114b32c1ab827f951a8a24def/skill"
    ],
    "type": [
      "VerifiableCredential"
    ],
    "issuanceDate": "2021-08-27T10:57:57.237Z",
    "credentialSubject": {
      "type": "Person",
      "name": "{{name}}",
      "trainedOn": "{{trainingTitle}}"
    },
    "issuer": "did:web:sunbirdrc.dev/vc/skill"
  },
  ...
}
```

* Ex: external url

```json
{
  ...
  "credentialTemplate": "https://gist.githubusercontent.com/tejash-jl/550aa1365c37e09065f1f134c936530d/raw/54f10668f8cad3953d9b09c09212d4ec4434ae7f/SkillExternalCredentialTemplate.json"
  ...
}
```

### Certificate Template

This property holds the template to be used for generating visual certificate. The key can be used while downloading the certificate.

```json
{
  ...
  "certificateTemplates": {
    "svg": "https://raw.githubusercontent.com/dileepbapat/ref-sunbirdrc-certificate/main/schemas/templates/TrainingCertificate.svg"
  },
  ...
}
```

### PrivateFieldConfig

The value for this property tells registry that how should privateFields configured in schema be emitted. This can have 5 different types of values.

There are 5 types of masking supported\
1\. NONE - field value will not be emitted\
2\. FULL - emit the field value as-is\
3\. HASH - emit the field as a one-way hash (salted)\
4\. MASK - emit the field using a masking strategy\
5\. HASH-MASK - emit two values for this field, one hashed and one masked

By default, both the configs hold NONE so the private and internal fields will not be emitted

```json
{
    ...
    "privateFieldConfig": "FULL",
    ...
}
```

### InternalFieldConfig

The value for this property tells registry that how should internalFields configured in schema be emitted. This can have 5 different types of values.

There are 5 types of masking supported\
1\. NONE - field value will not be emitted\
2\. FULL - emit the field value as-is\
3\. HASH - emit the field as a one-way hash (salted)\
4\. MASK - emit the field using a masking strategy\
5\. HASH-MASK - emit two values for this field, one hashed and one masked

By default, both the configs hold NONE so the private and internal fields will not be emitted

```json
{
    ...
    "internalFieldConfig": "FULL",
    ...
}
```

### Notification Templates

If you want to send out notifications, you can send it using this template configured in that particular schema. For each operation, you can have different templates. Also, for each operation, you can have multiple templates that you can send out.\
Following is an example of the notification templates

```
{
    ...
    "notificationTemplates": {
        "create": [{
            "subject": "Credential Created from schema",
            "body": "{\"sender\": \"AppName\",\"route\": \"4\",\"country\": \"91\",\"unicode\": 1,\"sms\": [{    \"message\": \"{{name}}, Your {{entityType}} credential has been created\",    \"to\": [      \"{{contact}}]\"    ]  
        }],
        "invite": [],
        "update": [],
        "delete": [],
        "revoke": []
    }
}
```

### UniqueIdentifierFields

If you want to auto generate some fields with specific format, you can generate those using this configuration in schema. each configuration of generatable field has 3 properties `field`, `idName`, `format` . Where field value should be path to the schema property you want to auto generate, idName is name with which you want to store the format, format is blueprint in which you want ids to be generated.\
Following is an example of uniqueIdentifierFields configuration

```
"_osConfig": {
  ...
  "uniqueIdentifierFields": [
    {
      "field": "/studentId",
      "idName": "student.id",
      "format": "ABN-[d{6}]"
    },
    {
      "field": "/classDetails/rollNumber",
      "idName": "class.details.roll.number",
      "format": "STD[SEQ_ROLLNUMBER]"
    }
  ],
  ...
}
```
