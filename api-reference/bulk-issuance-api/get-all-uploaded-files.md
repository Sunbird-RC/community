# Get all uploaded Files

<mark style="color:blue;">`GET`</mark> `/bulk/v1/uploads`

#### Headers

| Name          | Type   | Description                                                                                       |
| ------------- | ------ | ------------------------------------------------------------------------------------------------- |
| Authorization | String | Set to Bearer {access-token} if roles in schema is not anonymous. Else authorization can be empty |

{% tabs %}
{% tab title="200: OK With all the files that were uploaded" %}

{% endtab %}

{% tab title="403: Forbidden if the token is expired or you do not have appropriate permission to get uploaded files" %}

{% endtab %}
{% endtabs %}
