# Create Documents with Large Files

For files larger than XXMB (limit TBD) the recommended approach is to upload the file directly into DocFinity and then use a custom EDM end-point to index it. The complete process is below:

> **_NOTE:_** For brevity, all sample requests on this page excluded setting the authentication options.
> Please see [Getting Started](/docs/getting-started.md) for help on setting it.

## 1. Upload File

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
```
00000001fcpd3gb784gsfk7rftg9d5bq
```
The document id string is returned in the response body. Use this id for subsequent requests.

## 2. Index Document

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
