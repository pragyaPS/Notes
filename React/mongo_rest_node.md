### API - Application programming interface
- A server that creates an http interface for interacting with some data.
- Usually a server on some remote machine that dictaes how another application can interact with some data
- Basic data operation like CRUD

### Node js and APIs
- Node.js is javascript. It's async and event driven
- Single threaded (Can be optimized)
- when kept async, Node can handle a high amount of concurrent request
- Not great for CPU intensive work (data crunching, ML, big maths)

### Express

- The standard API framwork for node js
- Handles all tedious tasks like managing sockets, route matching, Error handling and more
- open source

### MongoDb

- the goto non-relational DB, works like dream in Node.js


### insomnia
- Powerful alternative tool of postman
- Free
- interact with the API
- has support for graphQL too

### middleware
- list of functions that executes before controller (callback function on app.get/post)
- Allows you to execute function on an incoming request with guaranteed order
- great for authenticating, transforming the req,tracking, error handling.


### REST routes with express

### Mongodb 

#### Document database
A record in mongodb is a document, which is a data structure composed of field and value pairs. MongoDb documents are similar to JSON objects. The values of field may include other documents, arrays, and arrays of documents.
{
    name: 'pragya',
    groups: ['news', 'sports']
}

### Schema for a schemaless DB

- Always us a schema for models(to avoid errors on frontend).  Mongoose makes it easy

MongoDB is a schemaless document store. but u should always use schemas, if you dont want to go crazy

MongoDB has added support for creating schemas but mongoose is much better









