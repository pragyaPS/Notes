### API - Application programming interface
- A server that creates an http interface for interacting with some data.
- Usually a server on some remote machine that dictaes how another application can interact with some data
- Basic data operation like CRUD(create, read, update, destroy)

### REST
- An API design that combines DB resources, route paths, and HTTP verbs to allow applications describe what action they are trying to perform
- Popularized when SaaS products starting offering APIs for integrations
- 

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

### custom middleware
the function pattern is same ass controller function. The difference is the intent. middleware intent is not to mutate the response and passon

```
const log = (req, res, next) => {
    console.log("logging");
    next(); // next runs the function stacked up for execution
    // not calling next would no
}
app.get('/data', log, (req, res) => {

})
The second parameter can be array of middleware as well i.e [log, log, log];
If we want to run the middleware to be called before each routing use app.use(log)

```


### REST routes with express

Express has designed keeping REST in mind and has all you need

- Express has a robust route matching system that allows for exact, regex, glob, and parameter matching
- It also supports HTTP verbs on a route based level. Together with the routing, you can create REST APIs
- Routes match in the order that they were defined (top to bottom)
- For abstraction, Express allows you to create sub routers that combine to make a full router

### routing 
Same as controller. We use it to have the route in separate tree

```
const router = express.Router();
router.get('/me', (req, res) => {
  res.send({message: 'this is from router me'})
})
app.use('/api', router)
``` 


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









