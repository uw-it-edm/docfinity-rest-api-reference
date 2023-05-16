# Get and Replace Document File

## Get Document Web Viewable (PDF) File

End-point that downloads the file as PDF for a given document id. 

`POST https://{host}/docfinity/servlet/repository`

Sample Request (form-data):

```bash
curl --location --request POST 'https://{host}/docfinity/servlet/repository' \
--header 'x-audituser: mynetid' \
--form 'id="00000001fcpd3gb784gsfk7rftg9d5bq"' \
--form 'pdf="true"' \
--form 'download="true"'
```

## Get Document Native File

End-point that downloads the native file from repository.

`POST https://{host}/docfinity/servlet/repository`

Sample Request (form-data):

```bash
curl --location --request POST 'https://{host}/docfinity/servlet/repository' \
--header 'x-audituser: mynetid' \
--form 'id="00000001fcpd3gb784gsfk7rftg9d5bq"' \
--form 'native="true"' \
--form 'download="true"'
```

## Replace Document File

Replacing a file associated with document is done in two steps: 
1. Upload the new file to DocFinity.
2. Execute a request to create a new version of the document pointing to the uploaded file id.

### 1. Upload the New File

Accepts a multipart/form-data request that accepts one file to upload.

`POST https://{host}/docfinity/webservices/rest/editing/replace/start`

Sample Request:

```bash
curl --location --request POST 'https://{host}/docfinity/webservices/rest/editing/replace/start' \
--header 'x-audituser: mynetid' \
--form 'file=@"/path/new-file.pdf"' \
--form 'json="1"'
```

Sample Response:

The identifier of the uploaded file, to be used in the next request.

```json
{"id":"00000001fdan7zjw3ck9j1ceg8aqvsra.pdf"}
```

### 2. Replace the File of Document

End-point that replaces the specified document with file that has been previously uploaded via the /replace/start service.

`POST https://{host}/docfinity/webservices/rest/editing/replace`

Sample Request:
```bash
curl --location --request POST 'https://{host}/docfinity/webservices/rest/editing/replace' \
--header 'x-audituser: mynetid' \
--header 'Content-Type: application/json' \
--data-raw '{
    "documentId": "00000001fb7b2p21aaayvdzvgbsvxfzs",
    "comments": "My change that describes the reason for replacement",
    "fileId": "00000001fea6r3tcf4rhmcxj1yfk5e9e.pdf",
    "fileName": "123.pdf"
}'
```

Sample Response:

Note that the response contains the id of the new version for the document.

```json
{
    "documentId": "00000001fb7b2p21aaayvdzvgbsvxfzs",
    "versionHistoryId": "00000001fdanaa2mha55fxmp4ytehtdb"
}
```
