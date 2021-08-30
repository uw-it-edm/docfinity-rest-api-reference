# Get Data about Document

## Get Document Attributes

`GET https://{host}/docfinity/webservices/rest/document/{documentId}`

Sample Response:

```json
{
  "id": "00000001fcpd3gb784gsfk7rftg9d5bq",
  "documentTypeId": "00000001fanar1rfjc94t8q4jac4xspb",
  "documentType": "My DocumentType",
  "categoryId": "00000001fanabm25h15ytsc4fbbna0k8",
  "category": "My Category",
  "displayName": "test-file.pdf",
  "createdBy": "test-user",
  "createdByFirstName": "FirstName",
  "createdByLastName": "LastName",
  "createdDate": 1628544549153,
  "indexedBy": "test-user",
  "indexedDate": 1628724736797,
  "indexed": true,
  "pointer": false,
  "deletedByFirstName": "",
  "deletedByLastName": "",
  "fileDefinitionType": "STANDARD",
  "retentionPolicyId": "00000000000000000000000000000001",
  "retentionPolicyName": "Undeclared Documents",
  "retentionPolicyOverridden": false,
  "canEdit": true,
  "canDelete": true,
  "canMarkup": true,
  "canView": true,
  "canViewMarkups": true,
  "canOverrideRedactions": true,
  "canList": true,
  "lastModifiedDate": 1628544549147,
  "entryMethod": "FILE_UPLOAD",
  "requiredIndexingMet": true,
  "redacted": false,
  "editPending": false,
  "resolutionLimit": 10000
}
```

## Get Document Metadata

`GET https://{host}/docfinity/webservices/rest/document/details?documentId={documentId}`

Sample Response:

```json
[
  {
    "metadata": {
      "id": "00000001fan88ra3fbntp94b50d6mkpt",
      "isCollection": false,
      "isCurrency": false,
      "decimalPrecision": 2,
      "metadataType": "STRING_VARIABLE",
      "name": "My Field",
      "currencyLocale": "en_US"
    },
    "folderTypeId": null,
    "values": [
      "Single-Select Value"
    ],
    "errored": false
  }
]
```
