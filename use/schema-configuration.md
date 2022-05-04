# Schema Configuration

## **Schema**

All registries have attributes pertaining to the entity or the fact in question either person or things. Schema defines the structure and constraints of the entity, open saber rc uses standard JSON-LD based schema.

In the example given below Place is Concept that registry is storing supporting name, city, addressRegion \(state\) and country.

#### Example:

```text
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

#### List of attributes

Collection of data values are also supported as multiple data value might represent the entity property for example subjects taught : \[“english”,”science”, “mathematics”\]

### Visibility scope:

3 types of visibility on attributes.

1. Public 
2. Private \(privateFields\)
3. Internal

Public data attributes are available in discovery by default any sort of permission / authorization is not needed by default.

Private attributes can be accessed by the owner by default, with consent 3rd party can access the data field.

Internal fields are system fields that only serve internal functionalities, these can never be accessed by any actors in the system.

#### Primary Keys for Entities

Primary keys from domain space will help enforce uniqueness of information and make it easily accessible. “uniqueIndexFields” can be used to configure the same.

Example:

`"uniqueIndexFields": [`

`"identityValue"`

`]`

### Index field set

Indexes can help in faster access to information, based on usage context index fields can be configured. Example:

`"indexFields": ["phoneNumber"],`

### Validation Extensions

JSON LD schema allows defining basic type constraints like numeric type, text or list of items etc. Also it allows a list of possible values for given attributes \(enum\) and regular expressions based constraints for the value.

### Add registry based on roles
Using `roles` config, one can set the authorization for add operation

Example:
let's say there are two registries Teacher and EducationCertificate, assume only Teacher can add the EducationCertificate, then assign role "teacher" to the relevant user in a keycloack and add the "roles" : ["teacher"] in the EducationCertificate config, now only token which has the role "teacher" can add the EducationCertificate

### Invite based on roles
Using `inviteRoles` config, one can set the authorization for invite operation

#### Example: 
##### 1. Only teacher can invite student
let's say there are two registries Teacher and Student, assume only Teacher can invite the Student, then assign role "teacher" to the relevant user in a keycloack and add the "inviteRoles" : ["teacher"] in the Student schema config, now only token which has the role "teacher" can invite the Student. 

##### 2. Anyone can invite student
Set "inviteRoles" : ["anonymous"] in Student schema config, now anyone can invite the Student


### Create login for registry
By default login is disabled for the registry, we need to set `enableLogin:true` inorder to create the user in keycloak.

### Attestation policy
List of policies can be configured for the attestation.

```
{
    "property": "educationDetails/[]",
    "paths": ["$.educationDetails[?(@.osid == 'PROPERTY_ID')]['instituteName', 'program', 'graduationYear', 'marks']", "$.identityDetails['fullName']"],
    "type": "MANUAL",
    "attestorEntity": "Teacher",
    "conditions": "(ATTESTOR#$.experience.[*].instituteOSID#.contains(REQUESTER#$.instituteOSID#) && ATTESTOR#$.experience[?(@.instituteOSID == REQUESTER#$.instituteOSID#)]['_osState']#.contains('PUBLISHED'))"
}
```
### Attestation Policy Attributes,
#### Property
It is used to pick up the specific property from the registry.
`"property": "educationDetails/[]"` this will pick up the educationDetails property for the attestation.
#### Paths
After attestation the system will store what are all the attributes are attessted, paths will basically helps us to pick those primary attributes which are used for the attestation and it gets saved in `_osAttestedData`
### Attestation Type
#### MANUAL
In this type two types of user will come into the picture, i.e Attestor and Requestor, Requestor will rise a claim, and the Attestor will approve/reject.
The authorization of the attestor will be defined in the config using `condition` attribute.

Example: Only teacher from the particular institue can attest the student, 
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
For this scenario the condition would be,
```conditions: "(ATTESTOR#$.experience.[*].instituteOSID#.contains(REQUESTER#$.instituteOSID#) && ATTESTOR#$.experience[?(@.instituteOSID == REQUESTER#$.instituteOSID#)"```
The above snippet says that attestor's education institue  and requestor's experience must belongs to the same institute.
