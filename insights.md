---
description: A brief overview of GraphQL components
---

# Insights

GraphQL has its own Type System using which we can describe data and implement the same in any framework.

GraphQL components:

* Schema
* Queries
* Mutations
* Resolver
* Introspection

### Schema

GraphQL schema is the core part which describes the objects, data types , relationships and any necessary information which can be used by client to access the data. This schema acts as template for clients to fetch data from the server, server uses this schema to validate and execute the client calls  and a client can get information about schema using Introspection operation.

Syntax to define a object in GraphQL

```graphql
type typename/objectname {
    field1 : field_type
    field2 : field_type
    .
    .
    }
```

{% hint style="info" %}
**type** : keyword

**typename/objectname** : User defined name for the object, For Example Student, Classroom, Faculty / but needs to start with root type which is `Query`which is the entry point and acts as path to access other types `Query` doesn't trigger a actual query

**field** : A data item related to application and underlying schema which is used by by client to identify the data, For example: student\_name, Student\_id

**field\_type** : Defines type of data stored in the fields, GraphQL has Scalar types \(Int, Float, String, Boolean, ID\), using aymbol ! at the end of field types will make the field to be non-empty
{% endhint %}

### Queries

### Mutations

### Resolver

### Introspection

This operation is used by client to get details about the GraphQL schema

