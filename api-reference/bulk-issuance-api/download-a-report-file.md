# Download a Report File

<mark style="color:blue;">`GET`</mark> `/bulk/v1/{id}/report`

#### Path Parameters

| Name                                 | Type   | Description                                |
| ------------------------------------ | ------ | ------------------------------------------ |
| id<mark style="color:red;">\*</mark> | String | Id of the report file you want to download |

#### Headers

| Name          | Type   | Description                                                                                       |
| ------------- | ------ | ------------------------------------------------------------------------------------------------- |
| Authorization | String | Set to Bearer {access-token} if roles in schema is not anonymous. Else authorization can be empty |

{% tabs %}
{% tab title="200: OK File will be downloaded with error response" %}

{% endtab %}
{% endtabs %}
