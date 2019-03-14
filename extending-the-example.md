---
description: Customizing the application to add more functionality
---

# Extending the example

### Search for a book

Earlier it searched all available books now we will customize it to search only for books we want based on title \(I am not using ID or ISBN as primary key.. it can also be done just minor modifications to schema\)

{% code-tabs %}
{% code-tabs-item title="src/index.js" %}
```javascript
type Query { 
    allbooks: [Book]
    findbookbytitle(title: String!):[Book]
  }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Add the resolver

{% code-tabs %}
{% code-tabs-item title="src/index.js" %}
```javascript
findbookbytitle: (root,{title}) => {
      return books.filter(books => {
        return books.title === title;
      })
    },
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![Searching Book by its title](.gitbook/assets/screenshot-from-2019-03-14-15-32-20.png)

{% hint style="success" %}
We can also extend the search based on different fields
{% endhint %}

Search Based on published year will look like this

```javascript
findbookbyyear(year: Int!):[Book]
```

and mutation for that will be as follows

```javascript
 findbookbyyear: (root,{year}) => {
      return books.filter(books => {
        return books.year === year;
      })
    }
```

![Find Books by published year](.gitbook/assets/screenshot-from-2019-03-14-15-33-42.png)

### Update a Book data

To update an entity first we need to find its index and update its data.

Define the mutation for update

{% code-tabs %}
{% code-tabs-item title="src/index.js" %}
```javascript
updatebook(title: String!, author: String, publisher: String, year:Int): Book   
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="danger" %}
If we don't check undefined value/if new value is not passed for all the fields  then Null assignment is done by default to respective field hence i used if statement
{% endhint %}

Resolver for update by title of the book

{% code-tabs %}
{% code-tabs-item title="src/index.js" %}
```javascript
updatebook: (root,args) => {
 const index = books.findIndex(books => books.title === args.title);
 if(args.title !== undefined )
 books[index].title = args.title;
 if(args.author !== undefined)
 books[index].author = args.author;
 if(args.publisher !== undefined)
 books[index].publisher = args.publisher;
 if(args.year !== undefined)
 books[index].year = args.year;
 return books[index]
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![Data Before Update](.gitbook/assets/screenshot-from-2019-03-14-15-19-09.png)

![Mutation changing data based on Book title](.gitbook/assets/screenshot-from-2019-03-14-15-22-45.png)

![New data After Update](.gitbook/assets/screenshot-from-2019-03-14-15-24-02.png)

### Delete a Book based by its title

This is also same process find the index of the book and delete it using `Splice()`

Define the mutation in the schema

{% code-tabs %}
{% code-tabs-item title="src/index.js" %}
```javascript
removebook(title: String!): Book         
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Define the resolver to delete a Book

{% code-tabs %}
{% code-tabs-item title="src/index.js" %}
```javascript
removebook: (root,args) => {
  const index = books.findIndex(books => books.title === args.title);
  const deletedbook = books[index];
  books.splice(index,1);
  return deletedbook;
 }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![Remove Book Mutation](.gitbook/assets/screenshot-from-2019-03-14-15-30-18.png)

![Data after deletion](.gitbook/assets/screenshot-from-2019-03-14-15-30-53.png)



