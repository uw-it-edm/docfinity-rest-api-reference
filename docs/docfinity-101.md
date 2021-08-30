# Understanding DocFinity Document Model

## Document Types and Category

A `Document Type` defines all the configuration for a kind of document to be stored in DocFinity. Including security access rules (who has access to documents of this type) and which metadata fields to store (which values to index and search by).

A `Category` is simply a way to group related document types together according to business needs. A document type must belong to a category.


## Metadata Objects

`Metadata Objects` are fields that can be assigned to a `Document Type` to capture information about the document and allow it to be searched by. Metadata Objects have a name (what the field is named in the UI) and a value set by the user. Some metadata objects can be set as required for a particular document type.

## Documents

`Documents` are the files stored in the DocFinity repository and have a unique identifier. Documents are assigned to a `Document Type`, which allows them to store values for each metadata object that is configured in the document type. Documents can be searched by its metadata values (also known as its indexed values).

## Document Indexing and Re-Indexing

To `Index` a document is the act of assigning values to (at least) all required metadata objects and committing it, which makes it available for search.

DocFinity has the ability to upload documents first and letting users index them later. `Un-Indexed` documents are files stored in the DocFinity server that have yet to be assigned to a `Document Type` or populated with its metadata values. They are accessible to the user who uploaded them but not discoverable by search.

To `Reindex` a document is the act of updating the values of one or more metadata objects.

## Document Attributes

In addition to metadata values, indexed `Documents` also have `Document Attributes`. These are common pieces of information stored for all `Documents` regardless of the `Document Type` assigned to it. Examples of document attributes are:

- "id"
- "displayName"
- "createdBy"
- "createdDate"
- "retentionPolicyName"
- "lastModifiedDate"
