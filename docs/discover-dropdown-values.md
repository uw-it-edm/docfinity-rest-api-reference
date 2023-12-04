# Discover Valid Values for Drop Down Fields

Knowing the valid values for dropdown fields is useful for integration clients that create and update documents. Values for dropdown fields are discovered by using the template search that has been configured for an integration client.

## For Drop Down Fields that Do Not Depend on Other Fields

Follow the [Search for Documents](/docs/document-search.md) guide to retrieve the prompt definitions for a search template.

Sample Request:
```bash
curl --location --request GET 'https://{host}/docfinity/webservices/rest/document/search/prompts/{searchID}/EXECUTION' \
--header 'x-audituser: mynetid' 
```

Sample Response (Redacted):

```json
[
  {
    "id": "00000001f8bgj6x8qjatn5rdsy2vze1c",
    "name": "Employee Group",
    "dataType": "STRING",
    "datasourceId": "00000001f8beq96hb9a7cgv0fv078eb1",
    "inputType": "LIST",
    "data": [
      {
        "value": "Academic",
      },
      {
        "value": "Staff",
      }
    ],
  }
]
```

Notice that the prompt containts a `data` property with an array of valid values.

## For Drop Down Fields that Depends on Other Fields

> __NOTE__: This functionality is not provided by default to all integration clients. If you have need for this use case, please contact [EDM Team](https://uw.service-now.com/sp?id=sc_cat_item&sys_id=07d54f351bbbd5d0cc990dc0604bcbff%20).

### Get Search Template Prompts

Follow the [Search for Documents](/docs/document-search.md) guide to retrieve the prompt definitions for a search template.

Sample Request:
```bash
curl --location --request GET 'https://{host}/docfinity/webservices/rest/document/search/prompts/{searchID}/EXECUTION' \
--header 'x-audituser: mynetid' 
```

Sample Response (Redacted):

```json
[
  {
    "id": "00000001fzgz3m2vs3zxt8kf0y7khc02",
    "name": "Record Category",
    "dataType": "STRING",
    "datasourceId": "00000001fzebn0js2szfvb79wc0e539r",
    "inputType": "LIST",
    "data": [
      {
          "value": "Letter"
      },
      {
          "value": "Appointment"
      }
    ],
    "responsibilityMapping": [
      "Record Type"
    ]
  },
  {
    "id": "00000001fzgz3m2x4zv1vrc0hwj9kd8b",
    "name": "Record Type",
    "dataType": "STRING",
    "datasourceId": "00000001fzebrne5rvtasqd794vde48x",
    "label": "Record Type",
    "inputType": "LIST",
    "data": [],
    "responsibilityMapping": [],
    "parameterPromptDatasourceArguments": [
      {
        "datasourceArgumentName": "Record Category",
        "argumentType": "PROMPT",
        "value": "Record Category"
      }
    ]
  }
]
```

### Execute Datasource for a Prompt

Notice in the previous response that the `Record Type` prompt does not return any values for its `data` property. This is because its values depend on the selection of the `Record Category` prompt. The dependencies of each prompt are described in the `parameterPromptDatasourceArguments` property.

Clients can retrieve the valid values for the `Record Type` prompt by executing its datasource, which can be identified by the `datasourceId` property.

End-point that executes a datasource by providing all its dependencies:

`POST https://{host}/docfinity/webservices/rest/document/datasource/{datasourceId}/execute`

Sample Request:

```bash
curl --location 'https://{host}/docfinity/webservices/rest/document/datasource/00000001fzebrne5rvtasqd794vde48x/execute' \
--header 'x-audituser: mynetid' \
--header 'Content-Type: application/json' \
--data '[
    {
        "name": "Record Category",
        "dataType": "STRING",
        "value": "Appointment"
    }
]'
```

Sample Response:

```json
[
    {
        "value": "Record Type 1"
    },
    {
        "value": "Record Type 2"
    },
    {
        "value": "Record Type 3"
    }
]
```