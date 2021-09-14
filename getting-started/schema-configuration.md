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

In addition to JSON LD specific data types OpenSaberRC supports various extension configurations under “\_osConfig”. Access, indexing, primary key etc are configurable using schema configuration.

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
