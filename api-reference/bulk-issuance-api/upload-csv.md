# Upload CSV

<mark style="color:green;">`POST`</mark> `/bulk/v1/{schemaName}/upload`

#### Path Parameters

| Name                                         | Type   | Description                                       |
| -------------------------------------------- | ------ | ------------------------------------------------- |
| <mark style="color:red;">\*</mark>schemaName | String | Schema for which you want to bulk create entities |

#### Headers

| Name          | Type   | Description                                                                                                                                                                                                                                                                     |
| ------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Authorization | String | <p>Set to Bearer {access-token} if roles in schema is not anonymous. Else authorization can be empty<br>* make sure roles env in bulk issuance service is configured <a data-mention href="../../developer-documentation/configuration.md#bulk-issuance">#bulk-issuance</a></p> |

#### Request Body

| Name                                   | Type                | Description |
| -------------------------------------- | ------------------- | ----------- |
| file<mark style="color:red;">\*</mark> | multipart/form-data | csv file    |

{% tabs %}
{% tab title="200: OK Information with ID of the file uploaded, number of success and failures" %}

{% endtab %}

{% tab title="403: Forbidden if the token is expired or you do not have appropriate permission to create entity" %}

{% endtab %}

{% tab title="500: Internal Server Error If invalid csv file is passed" %}

{% endtab %}
{% endtabs %}
