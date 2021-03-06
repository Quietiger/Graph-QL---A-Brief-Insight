---
description: Dissecting example - more info will be added later
---

# Anatomy of Example

To start with I am not going to add overhead of adding a Database server for now but instead I am going to use server session storage, A simple data is stored in index.js which will be accessed by application during its execution

{% code-tabs %}
{% code-tabs-item title="src/index.js" %}
```javascript
const books = [
  {
    title: "Learning GraphQL: Declarative Data Fetching for Modern Web Apps",
    author: 'Alex Banks and Eve Porcello',
    publisher: "O'Reilly Media",
    year: 2018
  },
  {
    title: 'Learning GraphQL and Relay',
    author: 'Samer Buna',
    publisher: "Packt Publishing",
    year: 2016
  },
  {
    title: 'React Cookbook: Create dynamic web apps with React using Redux, Webpack, Node.js, and GraphQL',
    author: 'Carlos Santana Roldan ',
    publisher: "Packt Publishing",
    year: 2018
  },
];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="warning" %}
Any Add/Delete/Update operations are temporary as storage is volatile and used only during run-time
{% endhint %}

Define Type Definitions which is the GraphQL schema, which defines Book, Query to fetch all books and also mutation to add a book\(this addition is temporary as storage is volatile\)

{% code-tabs %}
{% code-tabs-item title="src/index.js" %}
```javascript
const typeDefs = `
  type Query { books: [Book] }
  type Book { title: String, author: String, publisher: String, year:Int }
  type Mutation { addbook(title: String, author: String, publisher: String, year:Int): Book }
`;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Write resolvers to access schema and process queries

{% code-tabs %}
{% code-tabs-item title="src/index.js" %}
```javascript
const resolvers = {
  //Query the available books
  Query: { books: () => books }, 
  //Mutation to add new book
  Mutation: {   
      addbook: (parent,args)=> {
        //get data from arguments
          const Book = {
              title: args.title,
              author: args.author,
              publisher: args.publisher,
              year: args.year
          }
          books.push(Book) //Push data to GraphQL
          return Book
}
  }};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

now lets make put schema and resolvers together

{% code-tabs %}
{% code-tabs-item title="src/index.js" %}
```javascript
const schema = makeExecutableSchema({
  typeDefs,
  resolvers,
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now initialize the application, create an endpoint and start the sever

{% code-tabs %}
{% code-tabs-item title="src/index.js" %}
```javascript
// Initialize the app
const app = express();

// The GraphQL endpoint
app.use('/graphql', bodyParser.json(), graphqlExpress({ schema }));

// GraphiQL, a visual editor for queries
app.use('/graphiql', graphiqlExpress({ endpointURL: '/graphql' }));

// Start the server
app.listen(3000, () => {
  console.log('Go to http://localhost:3000/graphiql to run queries!');
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![Predefined Data for the application](.gitbook/assets/screenshot-from-2019-03-14-15-16-35.png)

![Mutation added new data](.gitbook/assets/screenshot-from-2019-03-14-15-18-32.png)

![](.gitbook/assets/screenshot-from-2019-03-14-15-19-09%20%281%29.png)

