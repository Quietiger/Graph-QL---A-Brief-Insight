# Simple Example

* Install NodeJS
*  Install Apollo Server
*  Install GraphQL - GraphQL tools, body parser,, etc..
*  Write GraphQL schema
*  Write resolver
*  Combine all features in Application
*  Execute the application and test

**Install NodeJS**

Download NodeJS : [https://nodejs.org/en/download/](https://nodejs.org/en/download/) 

Installing NodeJS : [https://github.com/nodejs/help/wiki/Installation](https://github.com/nodejs/help/wiki/Installation)

**Install Express and create project folder**

```bash
mkdir testapp
cd testapp
npm init
npm install express --save
```

{% hint style="warning" %}
_**`use sudo if permission is denied for installing a package`**_
{% endhint %}

_npm init_ creates npm package and adds a `package.json`to keep track of all the dependencies required for the application and you can also provide the following information accordingly during the creation

{% hint style="info" %}
1. `package name: (testapp)`
2. `version: (1.0.0) description:`
3. `entry point: (index.js)`
4. `test command:`
5. `git repository:`
6. `keywords:`
7. `author:`
8. `license: (ISC)`
{% endhint %}

**Installing Dependencies - Apollo and GraphQL to the application**

```bash
npm install apollo-server apollo-server-express --save
```

```bash
npm install express graphql graphql-tools body-parser --save
```

{% code-tabs %}
{% code-tabs-item title="package.json" %}
```javascript
{
  "name": "testapp",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "apollo-server": "^2.4.8",
    "apollo-server-express": "^1.4.0",
    "body-parser": "^1.18.3",
    "express": "^4.16.4",
    "graphql": "^14.1.1",
    "graphql-tools": "^4.0.4"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Create /Update src/index.js file with following code

{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
const express = require('express');
const bodyParser = require('body-parser');
const { graphqlExpress, graphiqlExpress } = require('apollo-server-express');
const { makeExecutableSchema } = require('graphql-tools');

// Defined Data
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

// The GraphQL schema in string form
const typeDefs = `
  type Query { books: [Book] }
  type Book { title: String, author: String, publisher: String, year:Int }
  type Mutation { addbook(title: String, author: String, publisher: String, year:Int): Book }
`;

// The resolvers
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

// Putting all together
const schema = makeExecutableSchema({
  typeDefs,
  resolvers,
});

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

save the file and start node by command `node index.js` and open browser and open URL [http://localhost:3000/graphiql](http://localhost:3000/graphiql)

This is what you get!

![](.gitbook/assets/screenshot-from-2019-03-13-18-06-29.png)

![](.gitbook/assets/screenshot-from-2019-03-13-20-07-07.png)



![](.gitbook/assets/screenshot-from-2019-03-13-20-07-31.png)

