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






