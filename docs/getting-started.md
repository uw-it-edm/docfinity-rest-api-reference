# Getting Started

## Environments

REST API is available in two environments, `Test` and `Prod` (resource URL's to be defined).

An early Dev access is available at `https://api.dev.concert.uw.edu/documents/` for testing and proof of concept.

## Authentication

REST API sits behind a proxy that validates the requests to it are authenticated. Requests are authenticated by sending a client certificate as part of the request. Please consult with the EDM team on how to obtain an access certificate.

## Authorization

The acess certificate will be tied to an account in DocFinity with appropriate security access for your team. All operations in DocFinity will have the access clearance of this account.

## Audit User Request Header

For auditing and tracking purposes, all request must include an `x-audituser` header set to a valid NetID. 

**Please note** that the requests are not made on-behalf of the audit user, they are made with the security access defined by the DocFinity account associated with the certificate, but the audit user will be logged in the operations audit tables of DocFinity and is meant for tracking information only.

## Preview Dev Access

For testing and proof of concept the REST API can be used without a certificate by passing an API KEY in the `x-api-key` header. Consult with EDM team to get a temporary API KEY to use.
