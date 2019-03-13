---
description: A brief overview of GraphQL components
---

# Insights

GraphQL has its own Type System using which we can describe data and implement the same in any framework. GraphQL has CRUD feature as it supports **C**reate **R**ead **U**pdate **D**elete feature to client for underlying database.

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
* **type** : keyword
* **typename/objectname** : User defined name for the object, For Example Student, Classroom, Faculty / but needs to start with root type which is `Query`which is the entry point and acts as path to access other types `Query` doesn't trigger a actual query
* **field** : A data item related to application and underlying schema which is used by by client to identify the data, For example: student\_name, Student\_id
* **field\_type** : Defines type of data stored in the fields, GraphQL has Scalar types \(Int, Float, String, Boolean, ID\), using symbol ! at the end of field types will make the field to be non-empty
{% endhint %}

### Queries

Client calls to GraphQL to fetch/read data from the server, which can be considered as a read operation from the client. When client makes a query operation GraphQL will validate the call against schema definition and after validation it fetches data from underlying database and returns data to client in JSON format.

Queries can vary from simple to complex nesting\(or depth\) based on types defined in the schema

{% code-tabs %}
{% code-tabs-item title="simple query" %}
```graphql
query
{
    typename1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="complex queries" %}
```graphql
query 
{
    typename1
        {    
            field_name
            typename2
                {
                    fieldname1
                    fieldname2
                }
            }
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Mutations

As we know GraphQL is CRUD styled and mutations are used to accomplish creating, updating and deleting operations from client side, the client has to send these operations to server via GraphQL and after validation and syntax check these operations are performed by GraphQL, but GraphQL will return the inserted data/modified data in JSON format once the operation is completed

Like Query Mutation is also a root type which acts as a root mutation or entry point for other Mutation types.

{% code-tabs %}
{% code-tabs-item title="Simple Mutation" %}
```graphql
type Mutation
{
    mutation_name(field1: field_type, field2: field_type) : returntype
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Mutation withnesting of objects" %}
```graphql
type Mutation
{
    mutation_name(field1: field_type, field2: field_type)
    {
        field1
        field2
            {
                field3
            }
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Resolver

### Introspection

This operation is used by client to get details about the GraphQL schema

