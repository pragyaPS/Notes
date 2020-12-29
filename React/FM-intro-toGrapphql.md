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
