# Create and Update Documents

Table of contents

1. [Custom EDM End-Points](#custom-edm-end-points)
1. [Request and Response Model](#request-and-response-model)
1. [Create a Document](#create-a-document)
1. [Create a Document For Testing (< 10MB)](#create-a-document-for-testing-%28%3C%2010MB%29)
1. [Update Metadata of a Document](#update-metadata-of-a-document)
1. [Metadata Types](#metadata-types)
1. [Clearing Metadata](#clearing-metadata)

## Custom EDM End-Points

To work around the limitations of DocFinity's core API, EDM team has developed custom end-points to create and update documents. *Note: url paths may change.*

- `https://{host}/documents/v1/commit`. End-point that indexes a file that has already been uploaded to DocFinity with the given metadata.
- `https://{host}/documents/v1/update`. End-point that updates the metadata of a document with the given values.
- `https://{host}/documents/v1/create`. For testing purposes: End-point that combines the process of uploading a file and indexing it with the given metadata.

Source code of custom end-points: [Document-API](https://github.com/uw-it-edm/document-api).

## Request and Response Model

Model used by request and responses `/create` and `/update` end-points:

```json
{
  "id": "DOC_ID",
  "category": "My Category",
  "documentType": "My Document Type",
  "metadata": [{
    "name": "My Field",
    "values": ["Single-Select Value"]
  }]
}
```

> **_NOTE:_** For brevity, all sample requests on this page excluded setting the authentication options.
> Please see [Getting Started](/docs/getting-started.md) for help on setting it.

## Create a Document

Creation of documents is a two step process, first the file is uploaded directly into DocFinity and then a request is made to index the file which will make it available for search.

### 1. Upload File

End-point accepts body as form-data with entries:

- json=1
- entryMethod=FILE_UPLOAD
- upload_files={/path/to/file.pdf}

`POST https://{host}/docfinity/servlet/upload`

Sample Request:

```bash
curl --location --request POST 'https://{host}/docfinity/servlet/upload' \
--header 'x-audituser: mynetid' \
--form 'json="1"' \
--form 'entryMethod="FILE_UPLOAD"' \
--form 'upload_files=@"/path/sample.pdf"'
```

Sample Response:

```text
00000001fcpd3gb784gsfk7rftg9d5bq
```

The document id string is returned in the response body. Use this id for the next request.

### 2. Index Document

End-point that indexes and commits a previously uploaded document.

`POST https://{host}/documents/v1/commit`

Sample Request:

```bash
curl --location --request POST 'https://{host}/documents/v1/commit' \
--header 'x-audituser: mynetid' \
--header 'Content-Type: application/json' \
--data-raw '{
  "id": "00000001fcpd3gb784gsfk7rftg9d5bq",
  "category": "My Category",
  "documentType": "My Document Type",
  "metadata": [{
    "name": "My Field",
    "values": ["My Value"]
  }]
}'
```

Sample Response:

```json
{
  "id": "00000001fcpd3gb784gsfk7rftg9d5bq",
  "category": "My Category",
  "documentType": "My Document Type",
  "metadata": [{
    "name": "My Field",
    "values": ["My Value"]
  }]
}
```

## Create a Document For Testing (< 10MB)

For testing purposes, this end-point combines the process of uploading a file and indexing it with the given metadata. Accepts a multipart/form-data request that accepts two files:

- documentFile: File to upload. (Maximum file size: **10MB**)
- metadataFile: File with JSON representation of the Document-API request model (see above) to index the document.

`POST https://{host}/documents/v1/create`

Sample Request:

```bash
curl --location --request POST 'https://{host}/documents/v1/create' \
--header 'x-audituser: mynetid' \
--form 'documentFile=@"/path/123.pdf"' \
--form 'metadataFile=@"/path/metadata-file.json"'
```

Sample 'metadataFile':

```json
{
  "category": "My Category",
  "documentType": "My Document Type",
  "metadata": [{
    "name": "My Field",
    "values": ["User Value"]
  }]
}
```

Sample Response:

```json
{
  "id": "00000001fcpd3gb784gsfk7rftg9d5bq",
  "category": "My Category",
  "documentType": "My Document Type",
  "metadata": [
    {
      "name": "My Field",
      "values": ["User Value"]
    },
    {
      "name": "Another Field",
      "values": ["Value set by server"]
    }
  ]
}
```

## Update Metadata of a Document

End-point to update the metadata values of a given document with the given updated values.
Metadata not provided in the call will retain its existing values.

`POST https://{host}/documents/v1/update`

> *Note*: Reindexing to a different category / document type is not currently supported.

Sample Request:

```bash
curl --location --request POST 'https://{host}/documents/v1/update' \
--header 'x-audituser: mynetid' \
--header 'Content-Type: application/json' \
--data-raw '{
  "id": "00000001fcpd3gb784gsfk7rftg9d5bq",
  "metadata": [{
    "name": "My Field",
    "values": ["New Value"]
  }]
}'
```

Sample Response:

```json
{
  "id": "00000001fcpd3gb784gsfk7rftg9d5bq",
  "category": "My Category",
  "documentType": "My Document Type",
  "metadata": [{
    "name": "My Field",
    "values": ["New Value"]
  }]
}
```

## Metadata Types

### Dates

Dates are represented in Unix epoch time, i.e. in milliseconds elapsed since January 1, 1970. For example:

```json
{
  "id": "00000001fcpd3gb784gsfk7rftg9d5bq",
  "metadata": [{
    "name": "My Field",
    "values": [1627282800000]
  }]
}
```

**Please Note** that DocFinity will truncate DateTime values to persist only the Date. For example: the value `1650040682000` will be truncated to `1650049200000`.

### Numbers

Integers and decimals are represented as javascript numbers. For example:

```json
{
  "id": "00000001fcpd3gb784gsfk7rftg9d5bq",
  "metadata": [
    {
      "name": "Integer Field",
      "values": [100]
    },
    {
      "name": "Decimal Field",
      "values": [100.99]
    }
  ]
}
```

### Multi-Select

Multi-select metadata objects only support strings, and they are represented as multiple entries in the values array. Note that for update operations, the server values of the field will be replaced by the values provided.

```json
{
  "id": "00000001fcpd3gb784gsfk7rftg9d5bq",
  "metadata": [{
    "name": "My Multi-Select Field",
    "values": ["First Value", "Second Value"]
  }]
}
```

## Clearing Metadata

For update operations, in order to delete the values of a metadata object set the values to `null`. This applies for all metadata types.

```json
{
  "id": "00000001fcpd3gb784gsfk7rftg9d5bq",
  "metadata": [{
    "name": "My Field",
    "values": null
  }]
}
```
