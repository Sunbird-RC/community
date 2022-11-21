# Upload A Template

To upload a html template, use following request

## Request

```http
POST /api/v1/{entity-type}/{entity-id}/templates/documents
```
| Field           | In     | Type      | Description                                          |
| --------------- | ------ | --------- | ---------------------------------------------------- |
| `content-type`  | header | `string`  | Set to `multipart/form-data`                            |
| `authorization` | header | `string`  | Set to `Bearer {access-token}`                       |
| `entity-type`   | path   | `string`  | Entity type for which the template will be uploaded                         |
| `entity-id`     | path   | `string`  | Entity ID for mentioned Entity Type
| `files`         | form-data   | `object`  | `files` is template-key which can be replaced with your choice. Requires a `html file` to be sent which will be uploaded

## Response

This will upload the template in minio and following object will be returned

```json
{
    "documentLocations": [
        "{url}"
    ],
    "errors": []
}
```
Important variables in the response body:

| Field                       | In   | Type     | Description                                                                                    |
| --------------------------- | ---- | -------- | ---------------------------------------------------------------------------------------------- |
| `documentLocations[0]` | body | `string` | The url of uploaded template. This value along with a prefix `minio://` is to be used in while retrieving an entity with content-type as `application/pdf`, `text/html`  |


## Usage

### cURL

```sh
curl --location \
    --header 'Authorization: {access-token}' \
    --form 'files=@"{file-path}"'
    --request POST 
    '{registry-url}/api/v1/Institute/1-453f7093-2da3-4fef-94ce-d7a19598e81d/templates/documents' \
```

### HTTPie

```sh
http --ignore-stdin \
    --form POST \
    '{registry-url}/api/v1/Institute/1-453f7093-2da3-4fef-94ce-d7a19598e81d/templates/documents' \
 'files'@{file-path} \
 Authorization:'Bearer {access-token}' \
```


> `{registry-url}` is usually http://localhost:{port}. The port can be found
> under the `registry` section in the `docker-compose.yml` file and is usually
> `8081`.
