# Get Sample Template

<mark style="color:blue;">`GET`</mark> `/bulk/v1/{schemaName}/sample-csv`

this will download a csv with the all fields that are needed to create entity for this schema

#### Path Parameters

| Name                                         | Type   | Description    |
| -------------------------------------------- | ------ | -------------- |
| schemaName<mark style="color:red;">\*</mark> | String | name of schema |

#### Headers

| Name          | Type   | Description                                                                                                                                                                                                                                                                  |
| ------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Authorization | String | <p>Set to <code>Bearer {access-token}</code> if roles in schema is not anonymous. Else authorization can be empty<br>* make sure ROLES env property has this role <a data-mention href="../../developer-documentation/configuration.md#bulk-issuance">#bulk-issuance</a></p> |

{% tabs %}
{% tab title="200: OK A CSV File with fields for header" %}

{% endtab %}

{% tab title="403: Forbidden if the token is expired or you do not have appropriate permission to create entity" %}

{% endtab %}

{% tab title="404: Not Found If schema is not found in the system" %}

{% endtab %}
{% endtabs %}
