# Getting Started

## Environments

REST API is available in two environments, `Test` and `Prod` (resource URL's to be defined).

An early Dev access is available at `https://api.dev.concert.uw.edu/documents/v1` for testing and proof of concept.

## Authentication

REST API sits behind a proxy that validates the requests to it are authenticated. Requests are authenticated by sending a client certificate as part of the request. Please consult with the EDM team on how to obtain an access certificate.

## Authorization

The acess certificate will be tied to an account in DocFinity with appropriate security access for your team. All operations in DocFinity will have the access clearance of this account.

## Audit User Request Header

For auditing and tracking purposes, all request must include an `x-audituser` header set to a valid NetID. 

**Please note** that the requests are not made on-behalf of the audit user, they are made with the security access defined by the DocFinity account associated with the certificate, but the audit user will be logged in the operations audit tables of DocFinity and is meant for tracking information only.

## Sample Request 

The client certificate and audit user header must be included in all requests.
```bash
curl --location --request POST 'https://{host}/{endpoint}' \
--header 'x-audituser: mynetid' \
--cert '/path/mycert.pem:mypassphrase' \
--key '/path/mycert.key' \
... 
```
