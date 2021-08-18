# Search for Documents

In DocFinity searches are configured by an administrator (also called `Template Searches`), they include settings for:

1. What fields a user may search for, called `prompts`.
1. What fields to display in the results.
1. What fields to sort the resuls by.

In order to execute a search via REST API, client must first gather the DocFinity identifier for the template search in order to populate a search request.

## Get Search Template ID

End-point that returns all searches that client has access to. From the response locate the id of the search to execute. Note that client can store this id as it will not change after the search template has been created by admin.

`GET https://{host}/docfinity/webservices/rest/search/getSearches`

Sample Response:

```json
[
  {
    "id": "00000001f8bgj6vs6zb7tg3vqaawqpay",
    "name": "College of Arts and Sciences",
    "description": "Description of search",
    "formDesignName": "",
    "formEditable": false,
    "searchSubSearches": [],
    "searchType": "TEMPLATE",
    "treeFiltersConfigured": false,
    "documentsExist": false,
    "promptsConfigured": true
  }
]
```

## Execute Search

End-point that executes a template search with the given prompt values. For a full

`POST https://{host}/docfinity/webservices/rest/document/search/template/execute`

Sample Request:

Note that the prompt names are the same as displayed in the search UI.

```json
{
  "searchId": "00000001fce4mzy01b3gtm3kqzq54tqt",
  "startIndex": 0,
  "maxNum": 50,
  "criteria": [
    {
      "attributeName": "Search Prompt 1",
      "strValue": "My Search Value"
    }
  ],
  "treeFilters": [],
  "searchSortOrders": []
}
```

Sample Response:

Note that each document entry has its main attribute information as well as the values for each of the display columns.

```json
{
  "results": [
      {
          "displayName": "Test Doc Name",
          "documentTypeId": "00000001fb79hcd7dt8faka6a8zfm276",
          "documentTypeName": "Library HR",
          "categoryName": "Library",
          "id": "00000001fcpmvd54hy0pet9zzcdy0mqj",
          "pointer": false,
          "rolesBitMask": 127,
          "thumbnailURL": "2021\\8\\9\\19\\00000001fcpmvd4ws981b5pt2stshgvt\\456.pdf",
          "notes": false,
          "fileSize": 85560,
          "fileDefinitionType": "STANDARD",
          "redacted": false,
          "hasLegalHold": false,
          "lastModified": 1628552672410,
          "columnValueDTOs": [
              {
                  "value": "1111",
                  "attributeName": "Reg ID",
                  "columnType": "METADATA",
                  "formatDateTime": false
              },
              {
                  "value": "Terry Chan",
                  "attributeName": "Display Name",
                  "columnType": "LINKED_OUTPUT_COLUMN",
                  "formatDateTime": false
              }
          ],
          "manuallyAdded": false
      }
  ],
  "maxResults": 50,
  "startIndex": 0,
  "totalAvailable": 58,
  "moreDataAvailable": true,
  "searchId": "00000001fce4mzy01b3gtm3kqzq54tqt",
  "searchType": "TEMPLATE",
  "searchDisplayName": "Library Search",
  "allowDocCreation": null,
  "subSearches": null,
  "fullTextResults": null,
  "report": false,
  "searchFormIntegration": null,
  "subSearchId": null,
  "sorts": null,
  "defaultSorts": null,
  "sortLabel": null,
  "paperClipId": null,
  "folderId": null,
  "isFolderOnLegalHold": false,
  "isSharedFolder": false,
  "folderShareType": null,
  "folderTypeId": null,
  "folderAccessType": null,
  "columnHeaderDTOs": [
      {
          "columnName": "Reg ID",
          "attributeType": "METADATA",
          "attributeName": "Reg ID",
          "attributeId": "00000001f8zje5ygjm86dfe6xd5ws8fy",
          "dataType": "STRING_VARIABLE",
          "columnType": "METADATA",
          "visibleFlag": true
      },
      {
          "columnName": "Display Name",
          "attributeType": "METADATA",
          "attributeName": "Reg ID",
          "attributeId": "00000001f8zje5ygjm86dfe6xd5ws8fy",
          "dataType": "STRING_VARIABLE",
          "columnType": "LINKED_OUTPUT_COLUMN",
          "visibleFlag": true
      }
  ],
  "totalPages": 2,
  "pageNum": 1
}
```

## (Optional) Get Search Template Prompts

End-point that returns all configured prompts for a search template. This is available to inspect the configuration for all prompts for a search.

`GET https://{host}/docfinity/webservices/rest/document/search/prompts/{searchId}/EXECUTION`

Sample Response:

```json
[
  {
    "allowMultipleValues": false,
    "id": "00000001f8bgj6x8qjatn5rdsy2vze1c",
    "name": "Employee Group",
    "dataType": "STRING",
    "datasourceId": "00000001f8beq96hb9a7cgv0fv078eb1",
    "label": "Employee Group",
    "inputType": "LIST",
    "data": [
      {
        "value": "Academic",
        "key": ""
      },
      {
        "value": "Staff",
        "key": " "
      }
    ],
    "maxLength": 50,
    "strDefaultValue": "Academic",
    "responsibilityMapping": [],
    "defaultValueType": "NONE",
    "staticDefaultValue": "Academic",
    "attributeType": "METADATA",
    "attributeName": "Employee Group",
    "required": false,
    "hidden": false,
    "editable": false
  }
]
```
