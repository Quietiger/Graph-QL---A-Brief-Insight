---
description: A experiment with Nesting
---

# Nested Queries...

Thought of implementing a 3 level nested query for GraphQL

Example: Start from Publisher, get books related to Publisher and get author information of the book. i.e., Get All the Books of the publisher with the author information of the respective books

{% code-tabs %}
{% code-tabs-item title="Publisher->Book->Author" %}
```javascript
{
allpublisher
  {
    id
    name
    books
    {
      title
      publisher
      year
      authors
      {
        id
        name
      }
    }
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

So Lets add a type Publisher and some data for the Publisher to the example, It can be done as follows

{% code-tabs %}
{% code-tabs-item title="Type Publisher" %}
```javascript
type Publisher{id: Int,name: String,books: [Book]}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Publisher Data" %}
```javascript
const publishers =[
  {
    id: 200,
    name: "Packt Publishing"
  },
  {
    id: 201,
    name: "O'Reilly Media"
  }
];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now write a Query and resolver for the field books in the publisher

{% code-tabs %}
{% code-tabs-item title="Query for Publisher" %}
```javascript
allpublisher: [Publisher]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Resolver for Publisher" %}
```javascript
allpublisher: () => { 
      return publishers;
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Resolving books field of Publisher type

{% code-tabs %}
{% code-tabs-item title="resolve books field" %}
```javascript
Publisher: {
      books(parent, args, ctx, info) {
  
          return books.filter(books => books.publisher === parent.name)
      }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now Publisher and Book got related, next is to relate Book and Author, Previously we related Author to book using name field of the author, But here we use author field of Book type to relate with Author

{% hint style="warning" %}
Apology for weak naming convention of fields
{% endhint %}

Add authors field of type Author to books and resolve that field later to get relationship to work as follows

{% code-tabs %}
{% code-tabs-item title="Book Type modification and resolving authors field of Book type" %}
```javascript
//.......
//.......
//Modify Query type for the Book

type Book { title: String, author: String, publisher: String, year:Int authors:[Author]}

 //.......
 //.......
 //Add Resolver for the authors field of Book
 
 
 Book: {
    authors(parent, args, ctx, info) {

        return authors.filter(authors => authors.name === parent.author)
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

And when you execute the file you should be able get output as shown below

![Publisher -&amp;gt; Book -&amp;gt; Author](.gitbook/assets/screenshot-from-2019-03-20-15-06-55.png)

As we can see now we have _**Packt Publishing's**_ id is 200 and they have _**2 books**_ under their publication and both of them are written by _**Samer Buna.**_

{% hint style="warning" %}
_**As we already related Author to Book we can again use nest for books under author. its ambiguous and confusing but still the relation works!!**_ 
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="Ambiguous" %}
```javascript
{
allpublisher
  {
    id
    name
    books
    {
      title
      publisher
      year
      authors
      {
        id
        name
        books
        {
          title
          year
        }
      }
    }
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

