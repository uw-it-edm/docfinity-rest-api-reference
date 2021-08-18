# Create and Update Documents

## Custom EDM End-Points

EDM team has developed two custom end-points to create and update documents that work around several deficiencies in the core DocFinity API's. *Note: url paths may change.*

- `https://{host}/documents/create`. End-point that combines the process of uploading a file and indexing it with the given metadata.
- `https://{host}/documents/update`. End-point that partially updates the metadata of given document.
- `https://{host}/documents/commit`. Alternate end-point to index a new document, intended for large files where client uploads the document separately.

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

## Create a Document

End-point that combines the process of uploading a file and indexing it with the given metadata. Accepts a multipart/form-data request that accepts two files:

- documentFile: File to upload.
- metadataFile: File with JSON representation of the Document-API request model (see above) to index the document.

`POST https://{host}/documents/create`

Sample Request:

```bash
curl --location --request POST 'https://{host}/documents/create' \
--form 'documentFile=@"/path/content-services-ui/123.pdf"' \
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

## Create Document with Large File

For large file uploads (upper limit TBD) see guide [Create Documents with Large Files](/docs/create-large-documents.md)

## Update Metadata of a Document

End-point that partially updates the metadata of given document.

`POST https://{host}/documents/update`

Sample Request:

```json
{
  "id": "00000001fcpd3gb784gsfk7rftg9d5bq",
  "metadata": [{
    "name": "My Field",
    "values": ["New Value"]
  }]
}
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

Dates are represented as milliseconds elapsed since January 1, 1970. For example:

```json
{
  "id": "00000001fcpd3gb784gsfk7rftg9d5bq",
  "metadata": [{
    "name": "My Field",
    "values": [1627282800000]
  }]
}
```

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
