# Create Documents with Large Files

For files larger than XXMB (limit TBD) the recommended approach is to upload the file directly into DocFinity and then use a custom EDM end-point to index it. The complete process is below:

## 1. Upload File

End-point accepts body as form-data with entries:

- json=1
- entryMethod=FILE_UPLOAD
- upload_files={/path/to/file.pdf}

`POST https://{host}/docfinity/servlet/upload`

Sample Request:

```bash
curl --location --request POST 'https://{host}/docfinity/servlet/upload' \
--form 'json="1"' \
--form 'entryMethod="FILE_UPLOAD"' \
--form 'upload_files=@"/path/sample.pdf"'
```

Sample Response:

The document id string is returned in the response body. Use this id for the next requests.

## 2. Index Document

End-point that indexes and commits a previously uploaded document.

`POST https://{host}/documents/commit`

Sample Request:

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

## 3. Delete File (optional)

If there is an error during indexing and the file is not needed anymore, a request can be made to this end-point to delete the file from the server.

`POST https://{host}/docfinity/webservices/rest/document/delete`

Sample Request:

Array of strings with the document identifier

```json
["00000001fcpd3gb784gsfk7rftg9d5bq"]
```
