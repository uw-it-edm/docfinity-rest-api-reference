# Create and Update Documents

## Custom EDM End-Points

To work around the limitations of DocFinity's core API, EDM team has developed custom end-points to create and update documents. *Note: url paths may change.*

- `https://{host}/documents/v1/create`. End-point that combines the process of uploading a file and indexing it with the given metadata.
- `https://{host}/documents/v1/update`. End-point that updates the metadata values of a given document with the given updated values.
- `https://{host}/documents/v1/commit`. Alternate end-point to index a new document, intended for large files where client uploads the document separately.

Source code of custom end-points: [Document-API](https://github.com/uw-it-edm/document-api).

## Request & Response Model

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

## Create a Document (< 10MB)

End-point that combines the process of uploading a file and indexing it with the given metadata. Accepts a multipart/form-data request that accepts two files:

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

## Create Document with Large File (> 10MB)

For large file uploads (> 10MB) see guide [Create Documents with Large Files](/docs/create-large-documents.md)

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
