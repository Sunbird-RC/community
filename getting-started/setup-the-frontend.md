# Setup the Frontend

### Installation

This is an Angular 8 project, and you need to install the dependencies, and run the project.

### Configuration

The application needs to be configured with the appropriate fields to be able to use it. Example configuration is provided in the `src/examples` folder.

#### Environment Config

| Key | Value |
| :--- | :--- |
| `baseUrl` | Base URL for the OpenSaber backend. Eg: [https://registry.com/api](https://registry.com/api) |
| `schemaUrl` | URL to the OpenAPI schema definition. This could be a HTTP path or a path to a local file Eg: [https://registry.com/api/schema.json](https://registry.com/api/schema.json) OR /assets/schema.json |
| `logo` | URL to logo. This logo is displayed in the header of the UI |

#### Forms

The `forms.json` needs to be placed in `src/assets/config`. This file defines the schema for various forms used, along with the fields for each. The form rendering is based on the formly.dev library, and the forms.json is a small wrapper on top of the formly schema.

In this file `forms` is an array with key/value pairs. They key is the code / slug of the form which is used to access the form. Eg: if the key for a form is `employee-signup` that form can be accessed via `/forms/employee-signup`. Each form definition will have the below fields -

| Key | Value |
| :--- | :--- |
| `form.api` | This is the path to the API endpoints for the entity this form handles. Eg: `/Employer` |
| `form.type` | Forms can be of 2 types. It can either be a form to create a new entity Eg: Employer, or it could be a form to submit a "sub-field" eg: work experience of an employee. For the former use `entity`. For the latter use `property:<property name>` \(eg: property:work\_experience\) |
| `form.formclass` | HTML Class applied to the form container |
| `form.title` | Title of form |
| `form.redirectTo` | Redirect URL on after form submit |
| `form.fieldsets` | List of fieldsets\(multiple\) for this form. At least one fieldset is needed |

**fieldsets**

| Key | Value |
| :--- | :--- |
| `fieldsets.definition` | Name of the OpenAPI "Definition" to use |
| `fieldsets.fields` | List of fields\(multiple\) to populate for this fieldset. If you wish to display all fields from the schema, you can skip defining each field, and use use `"fields": ["*"]` |

**fields**

| Key | Value |
| :--- | :--- |
| `fields.name` | Name of field \(same as defined in defination of that schema\) |
| `fields.custom` | `boolean` Name of custome field \(not defined in defination of that schema\) |
| `fields.required` | `boolean` |
| `fields.class` | Class of field |
| `fields.disabled` | `boolean` Disable the field \(readonly\) |
| `fields.children` | `object` Reference field of defination \(same properties as `fieldsets`\) |
| `fields.validation` |  |

#### Layouts

The `layouts.json` is used to define how the public and private profile pages look like. For each entity in OpenSaber backend, a layout file should be defined with the fields and the order in which they should display.

In this file `layouts` is an array with key/value pairs. They key is the code / slug of the layout page which is used to access the form. Eg: if the key for a layout is `employee-profile` that page can be accessed via `/profile/employee-profile`. Each layout definition will have the below fields -

| Key | Value |
| :--- | :--- |
| `layout.api` | URL Path of API |
| `layout.title` | Title of form |
| `layout.blocks` | Cards/Blocks \(multiple\) to populate in `layout`. |

**blocks**

| Key | Value |
| :--- | :--- |
| `blocks.definition` | Defination of fields from JSON Schemas in `schemaUrl` |
| `blocks.title` | Title of Card/Block |
| `blocks.add` | `boolean` Enable Add Button |
| `blocks.addform` | `<name of form from forms>` Form opens on Add Button click |
| `blocks.edit` | `boolean` Enable Edit Button |
| `blocks.editform` | `<name of form from forms>` Form opens on Edit Button click |
| `blocks.multiple` | `boolean` Enable Multiple values |
| `blocks.fields` | Array/List of fields\(multiple\) to populate in `fieldsets` |

**fields**

| Key | Value |
| :--- | :--- |
| `fields.includes` | Array/list of Included Fields from response or `[*]` for all fields |
| `fields.excludes` | Array/list of Excluded Fields from response |

## FAQs

### Proxy configuration

To avoid CORS issues you can use proxy configuration. Run `npm start` or `ng serve --proxy-config proxy.conf.json`. For additional configuration please check `proxy.conf.json` file.

### Hosting the Frontend

The frontend may be hosted any of the below ways

* As a container. You may create an image with the angular build files. 
* On a VM
* In blob storage \(eg: S3, with a CDN in front\)

