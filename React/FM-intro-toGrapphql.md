# Graphql
- new technology sitting in a new place.
REST is a way to build an API whereas graphql sits in front of an API

- GraphQL only has one URL. It does not need a resource url + verb combo. The request details are in a POST body (sometimes GET)
- In REST, the shape and size of the data resource is determined by the server, with Graphql its determined by the query (request)
- In REST, you have to make multiple API calls to retrieve relational data, with GraphQL you can start with entry resource and traverse all the connections in one request
- In REST, the shape of the response is determined by whom ever created the server, in GraphQL the shap is determined by the query
- In REST, a single request will execute on controller on the server, in GraphQL a request might execute MANY resolvers on the server
many more....

**Nodemon** same as node except it watches the file.

## Apollo server

**Apollo Server** is an open-source, spec-compliant GraphQL server that's compatible with any GraphQL client, including Apollo Client. It's the best way **to build a production-ready, self-documenting GraphQL API that can use data from any source.**

You can use Apollo Server as:
- A stand-alone GraphQL server, including in a serverless environment
- An add-on to your application's existing Node.js middleware (such as Express or Fastify)
- A gateway for a federated data graph

Apollo Server provides:
- Straightforward setup, so your client developers can start fetching data quickly. 
- Incremental adoption, allowing you to add features as they're needed
- Universal compatibility with any data source, any build tool, and any GraphQL client
- Production readiness, enabling you to ship features faster

        const {  ApolloServer, gql } = require('apollo-server');
        const typeDefs = gql`
        type Book {
            title: String,
            author: String
        }
        type Query {
            books: [Book]
        }`;

This snippet defines a simple, valid GraphQL schema. Clients will be able to execute a query named books, and our server will return an array of zero or more Books.

#### Define your data set
Now that we've defined the structure of our data, we can define the data itself. Apollo Server can fetch data from any source you connect to (including a database, a REST API, a static object storage service, or even another GraphQL server).
lets assume this simple data set that clients can query. 

    const books = [
    {
        title: 'The Awakening',
        author: 'Kate Chopin',
    },
    {
        title: 'City of Glass',
        author: 'Paul Auster',
    },
    ];

#### Define a resolver
 but Apollo Server doesn't know that it should use that data set(Array of book) when it's executing a query. To fix this, we create a **resolver**.

 **Resolvers** tell Apollo Server how to fetch the data associated with a particular type. Because our Book array is hardcoded, the corresponding resolver is straightforward.

    // Resolvers define the technique for fetching the types defined in the
    // schema. This resolver retrieves books from the "books" array above.
    const resolvers = {
    Query: {
        books: () => books,
    },
    };

#### Create instance of `ApolloServer`

We've defined our schema, data set, and resolver. Now we just need to provide this information to Apollo Server when we initialize it.

    // The ApolloServer constructor requires two parameters: your schema
    // definition and your set of resolvers.
    const server = new ApolloServer({ typeDefs, resolvers });

    // The `listen` method launches a web server.
    server.listen().then(({ url }) => {
    console.log(`🚀  Server ready at ${url}`);
    });

#### start the server

`node index.js`


Apollo server loads up graphql playground on server start

### We already have a DB Schema

- DB Schema is for keeping data consistent when entering our DB
- GraphQL schema is for defining what resources are available for querying, how they relate, and how you can query them
- Both schemas can be the same, or not. DB schema is a good starting point for your GraphQL schema
- GraphQL schema sits in front of your DB queries, it validates incoming request queries.
- Some GraphQL tools create GraphQL APIs based off of your DB Schemas, and the other way around

### Creating schemas with SDL
 Many ways to create schemas, SDL is the best

- Schema Definition Language (SDL)
- Instead of using functions to create a schema, use a verbose, string based syntax for your schemas. Later you can transform that syntax into many other representations if needed
- Much easier to read
- Can be composable
- Supported by all tools 

#### Schema cheatsheet

https://raw.githubusercontent.com/sogko/graphql-shorthand-notation-cheat-sheet/master/graphql-shorthand-notation-cheat-sheet.png

### Scalar and Object types

Describe resources that will be used in queries and mutations
Scalar types are built in primitives
- String
- Int
- Float
- Boolean
- ID

- Object types are custom shapes with fields that either scalar types or other object types
- Object type fields also describe any arguments and or validations
- Types are the target or all requests

### Query and Mutation types

CRUD on your GraphQL API

- Query type describes the different queries your API is capable of.
- A query operation is just a name, with possible arguments that eventually returns a type
- A mutation is the exact same thing, but with the intent of mutating the DB vs just reading
- Queries and Mutations are what will be available to clients interacting with your API, think of them as your routes
- Input is same as type the intent is to take the input

### Resolvers

What are resolvers?
- Resolvers are like controllers in a REST API. They are responsible for retrieving data
- Every query and mutation your schema has, must have a resolver that returns the specified type
- Types and fields on types often have resolvers as well
- Incoming query dictates what resolvers run and in what order

Apollo Server needs to know how to populate data for every field in your schema so that it can respond to requests for that data. To accomplish this, it uses resolvers.

A **resolver** is a function that's responsible for populating the data for a single field in your schema. It can populate that data in any way you define, such as by fetching data from a back-end database or a third-party API.