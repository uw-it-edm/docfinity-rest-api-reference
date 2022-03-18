# Delete Documents

If your API account has been granted delete access to a Document Type, the following end-point can be used to delete a document assigned to that Document Type.

Note that this operation performs a "soft-delete", the document will still be in DocFinity but no longer available for viewing nor included in search results. Periodically, DocFinity will completely purge deleted documents.

`POST https://{host}/docfinity/webservices/rest/document/delete`

Sample Request:
```bash
curl --location --request POST 'https://{host}/docfinity/webservices/rest/document/delete' \
--header 'x-audituser: mynetid' \
--header 'Content-Type: application/json' \
--data-raw '["00000001fcpd3gb784gsfk7rftg9d5bq"]'
```

The body of the request is an array of strings with the document identifier(s)

Response: </br>
A response code of `204 No Content` indicates the document(s) have been deleted successfully.
There is no content in the return body.
