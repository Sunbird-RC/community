---
description: >-
  Sunbird RC provides a functionality to transform the entity response body with
  different transformation templates
---

# View Templates Configuration

In SunbirdRC we can configure view templates (JSON transformers) which can be applied during runtime. It supports enabling/disabling properties in JSON responses. It also provides executing simple [JEXL](https://commons.apache.org/proper/commons-jexl/) expressions and also supports creating custom provider functions.

Below is an example of a view template:

```json
{
  "id": "personDefaultView1",
  "subject": "Person",
  "fields": [
    {
      "name": "firstName",
      "title": "NAME"
    },
    {
      "name": "lastName",
      "display": true
    },
    {
      "name": "nationalIdentifier",
      "title": "OS number",
      "display": false,
      "$comment": "This field is not displayable, but needed for internal referencing"
    },
    {
      "title": "Name in passport",
      "function": "#/functionDefinitions/concat($lastName, $firstName)",
      "$comment": "This is a virtual field not defined in the schema"
    },
    {
      "title": "Name as in DL",
      "function": "#/functionDefinitions/userDefinedConcat($firstName, $lastName)",
      "$comment": "This is a virtual field not defined in the schema"
    }
  ],
  "functionDefinitions": [
    {
      "name" : "concat",
      "result": "arg1 + \", \" + arg2",
      "$comment": "arg1 and arg2 will be populated with parameter values at runtime"
    },
    {
      "name" : "userDefinedConcat",
      "provider": "dev.sunbirdrc.provider.SampleViewFunctionProvider",
      "$comment" : "Complex operations that cannot be expressed easily in an in-line function definition can be implemented as a class. "
    }
  ]
}
```

When the above template is applied for the below response payload:

```json
{
  "Person": {
    "nationalIdentifier": "nid823",
    "firstName": "Ram",
    "lastName": "Moorthy",
    "gender": "MALE",
    "dob": "1990-12-10"
  }
}
```

The response will be transformed to:

```json
{
  "Person": {
    "NAME": "Ram",
    "lastName": "Moorthy",
    "Name in passport": "Moorthy, Ram",
    "Name as in DL": "Ram : Moorthy"
  }
}
```

The view templates support configuring what fields should be displayed, changing the field names, and also performing functionalities on top of the fields.&#x20;

In the above template, a field called `firstName` which is present in the entity is been renamed to `NAME.`

```json
{
      "name": "firstName",
      "title": "NAME"
}
```

By default, all fields defined in the template are treated to be `"display": true`. If the fields  are not supposed to be displayed then we can set `"display": false`

```json
{
      "name": "nationalIdentifier",
      "title": "OS number",
      "display": false,
      "$comment": "This field is not displayable, but needed for internal referencing"
}
```

We can also define expressions or custom functions to perform complex/custom transformations.

```json
{
      "name" : "concat",
      "result": "arg1 + \", \" + arg2",
      "$comment": "arg1 and arg2 will be populated with parameter values at runtime"
},
```

In the above example, a custom JEXL expression to concatenate two fields is inline defined in the view template.&#x20;

```json
{
      "title": "Name in passport",
      "function": "#/functionDefinitions/concat($lastName, $firstName)",
      "$comment": "This is a virtual field not defined in the schema"
},
```

The above code illustrates on how to call the inline-defined functions for a specific field.

We can also write Java code which can be used to perform custom/complex operations and also execute that in the view template.

```json
{
      "name" : "userDefinedConcat",
      "provider": "dev.sunbirdrc.provider.SampleViewFunctionProvider",
      "$comment" : "Complex operations that cannot be expressed easily in an in-line function definition can be implemented as a class. "
}
```

The implementation of the above provider is here, [https://github.com/Sunbird-RC/sunbird-rc-core/blob/3dcca8a7ca6be355991808a0d1ebfc3a824c43c5/java/view-templates/src/main/java/dev/sunbirdrc/provider/SampleViewFunctionProvider.java](https://github.com/Sunbird-RC/sunbird-rc-core/blob/3dcca8a7ca6be355991808a0d1ebfc3a824c43c5/java/view-templates/src/main/java/dev/sunbirdrc/provider/SampleViewFunctionProvider.java)

A custom provider function should implement `IViewFunctionProvider`&#x20;

Similarly, SunbirdRC is shipped with another provider functions that can be used for transformation,\
[RemovePathFunctionProvider](https://github.com/Sunbird-RC/sunbird-rc-core/blob/3dcca8a7ca6be355991808a0d1ebfc3a824c43c5/java/view-templates/src/main/java/dev/sunbirdrc/provider/RemovePathFunctionProvider.java)\
The above provider function will remove JSON paths from a nested object.\
Example to demonstrate how to use the above function,

```json
{
  "subject": "Reporter",
  "fields": [
    {
      "name": "name",
      "title": "name",
      "display": true
    },
    {
      "name": "address",
      "title": "address",
      "function": "#/functionDefinitions/removePath($address, $.line)",
      "display": true
    }
  ],
  "functionDefinitions": [
    {
      "name" : "removePath",
      "provider": "dev.sunbirdrc.provider.RemovePathFunctionProvider"
    }
  ]
}
```

The above template will transform the below response payload

```json
{
  "Person": {
    "nationalIdentifier": "nid823",
    "name": "Ram",
    "lastName": "Moorthy",
    "gender": "MALE",
    "dob": "1990-12-10",
    "address": {
      "line": "1st stree",
      "city": "bangalore"
    }
  }
}
```

to the below format

```json
{
  "Person": {
    "name": "Ram",
    "address": {
      "city": "bangalore"
    }
  }
}
```

The view templates can be applied to both the GET APIs. We need to pass a `viewTemplateId` the header which contains the value of the view template file to be applied.

{% content-ref url="../api-reference/registry/get-an-entity.md" %}
[get-an-entity.md](../api-reference/registry/get-an-entity.md)
{% endcontent-ref %}

{% content-ref url="../api-reference/registry/get-an-entity-by-id.md" %}
[get-an-entity-by-id.md](../api-reference/registry/get-an-entity-by-id.md)
{% endcontent-ref %}

Example:

```powershell
curl --location 'localhost:8081/api/v1/StudentWithPassword/1-2ee0c034-0a81-4c7f-a971-058af35911cb' \
--header 'viewTemplateId: student_view_template.json' \
```

View templates can be used in search/discovery APIs. The respective template id needs to be part of the request body as below

```json
{
    "filters": {

    },
    "viewTemplateId": "student_view_template.json"
}
```

{% content-ref url="../api-reference/discovery-api/search-an-entity.md" %}
[search-an-entity.md](../api-reference/discovery-api/search-an-entity.md)
{% endcontent-ref %}
