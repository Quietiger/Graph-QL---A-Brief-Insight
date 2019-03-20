---
description: Simple Nesting of Queries
---

# Nested Queries

Any data you want to get from GraphQL need to have a Query associated by a resolver.

If we want to perform some queries like below then we have do some modification to our example and try to get books written by respective  author like below

{% code-tabs %}
{% code-tabs-item title="Nested Query" %}
```javascript
{
  allauthors{
    id
    name
    books{
      title
      author
      year
      publisher
    }
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



unlike previous function call `findbookbyauthor(author:"Samer Buna")` which searched only books for author name we modify it to get both author information and also books written by that author by using **author\(name: String!\)** query, simply we can say that you query author and all of his books in one query.

we add some data of authors as constant in the code

{% code-tabs %}
{% code-tabs-item title="Author data" %}
```javascript
const authors=[
{
  id:100,
  name:"Alex Banks"
},
{
  id:101,
  name:"Eve Porcello"
},
{
  id:102,
  name:"Samer Buna"
},
{
  id:103,
  name:"Carlos Santana Roldan"
}
];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="warning" %}
for now we implement search \(A Strict Searching\) for exact string hence we need to do some modifications to our books data as follows
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="Books Data" %}
```javascript
const books = [
  {
    id:1,
    title: "Learning GraphQL: Declarative Data Fetching for Modern Web Apps",
    author: 'Eve Porcello',
    publisher: "O'Reilly Media",
    year: 2018
  },
  {
    id:1,
    title: "Learning GraphQL: Declarative Data Fetching for Modern Web Apps",
    author: 'Alex Banks',
    publisher: "O'Reilly Media",
    year: 2018
  },
  {
    id:2,
    title: 'Learning GraphQL and Relay Part 1',
    author: 'Samer Buna',
    publisher: "Packt Publishing",
    year: 2016
  },
  {
    id:2,
    title: 'Learning GraphQL and Relay Part 2',
    author: 'Samer Buna',
    publisher: "Packt Publishing",
    year: 2018
  },
  {
    id:3,
    title: 'React Cookbook: Create dynamic web apps with React using Redux, Webpack, Node.js, and GraphQL',
    author: 'Carlos Santana Roldan',
    publisher: "Packt Publishing",
    year: 2018
  },
];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

then we modify the types as follows \(add a field books with type Book\)

{% code-tabs %}
{% code-tabs-item title="modify author type" %}
```javascript
type Author{id: Int,name: String,books: [Book]} 
```
{% endcode-tabs-item %}
{% endcode-tabs %}

write a resolver for the author as below

{% code-tabs %}
{% code-tabs-item title="Author Query" %}
```javascript
allauthors: () => { 
      return authors;
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now we need to resolve the books field of type Book in the Author type so we need a resolver as below

{% code-tabs %}
{% code-tabs-item title="Reolve books field" %}
```javascript
Author: {
    books(parent, args, ctx, info) {
        console.log(parent)
          //parent is the root object (User is the parent here)

        return books.filter(books => books.author === parent.name)
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

and when we execute the file we should get the following output

![Output Author and Book by that author](.gitbook/assets/screenshot-from-2019-03-20-13-18-46.png)

{% hint style="info" %}
Try modifying the data and check working of the nested queries
{% endhint %}

