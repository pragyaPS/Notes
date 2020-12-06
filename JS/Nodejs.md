Intro to nodeJS
https://slides.com/scotups/deck/fullscreen#/2/0/3

Environment to run javascript outside a browser
- open source runtime
- built on chrome's v8 Javascript Engine
- Created by Ryan dhal in 2009
- Evolved since its created to allow developers to build almost anything

### What can I create with Nodejs?

Pretty much anything a scripting and server language like python or ruby can, but with JavaScript
- Tooling (build, automation, etc)
- API's (REST, Realtime, etc)
- CDNs
- Shareable libraries
- Desktop applications
- Pretty much anything because node is on everything now

### Browser vs Nodejs

| Browser  | Nodejs  |
|---|---|
|Build interactive apps for the web |Build server side apps & scripts|
|  DOM |  Nope, No GUI (Can virtuallize) |
|  Window | No window, but does have global  |
|  Fragmentation | Versioned, so no fragmentation  |
|  optional modules (es6) | Required modules (commonjs +)  |
|  Cannot access filesystem | Can access filesystem |
|  Async | Async No browser based APIs |


### GLobals
Nodejs gives you helpful globals, but just like the browser, you should not create your own
- Process: has information about the environment the program is running in
- require: function to find and use modules in current module
- __dirname: The current directory path 
- module - information about current module, methods for making module consumable
- global - like window, its the "global" object. Almost NEVER use this
...many more

### What are modules?

    var module1 = (function(exports, require, module, __filename, __dirname){
    // your node js code in a file
    })

    var module2 = (function(exports, require, module, __filename, __dirname){
    // your node js code in another file
    })


### Modules in Nodejs

NodeJs uses commonJs for its module system.


There are other module systems out there like:

ESM (ecmascript modules) *new standard
AMD (pretty much dead)
... others, but don't matter at all

- Dont use `exports`. Always use `module.exports =`
- ```const nameFn = require('./lib.js') // put ./ if its created by```
- Mostly app has one entrypoint which resolves everything . and then it creates dependency graph


### npx
downloaded with node. Added recently in  the package. It lets you run local cli globally

### fileSystem (fs)
the difference between reading file from filesystem and requiring it as module is 
require gives executed code whereas fs.readfile gives back string 

    const fs = require('fs');
    // Reading a file
    const file = fs.readFileSync('./lib.js', {encoding: 'utf-8'}).toString()
    // Writing a file
    fs.writeFileSync('./lib.js', 'var me = "Pragya"');


### Shipped modules
Nodejs ships with a bunch of helpful modules already

- fs - fileSystem module to interacting with files on a machine
- http - low level-ish module for creating network based programs, like APIs
- path - useful for manipulating path strings and handling differences across many OS's
... many more

    const path = require('path');

use relative paths only in require. rest places use path 

### Remote modules

download and use other modules from the internets 🙀

Nodejs has grown a bunch, and a bunch of that growth is due to its community and the ability to share modules and consume them at will. 

tldr; download and use other modules from the internets 🙀

You can slap together an app really fast by reusing public modules. Which are the same as the modules you make, but packaged for downloading

This sounds nice, but now you have to be aware of malicious code. Also, you need a system to help with the management of remote modules (downloading, publishing, updating, etc)


### Three module types, one require

modules you created are always relative paths. ".js" is implied

**Custom local modules**
    var lib = require('../rel/path/to/lib') // Always have to use a "." first
**Remote modules**
    var lib = require('lib') // the same name you used to install it with npm  

**Shipped modules**
    var fs = require('fs') // internal module, remote module with same name takes it

### NPM

CLI to manage remote modules

Ships with Nodejs
- Allows you to publish, download, and update modules
- Uses package.json file in your Nodejs project to determine dependencies
- Stores remote modules in the node_modules folder

### Json to Object
```const obj = JSON.parse(json);```

### Async code

Nodejs is single threaded and event based. Just like the browser. 

tldr; Nodejs is single threaded and async like the browser, but you'll probably do more async things

Unlike the browser, your nodejs app will be shared by all clients 

You now have consider CPU intensive tasks that block the single thread (while loops)


### Async patterns

async / await is legit

Callback pattern

    // callback takes error as first arg, and result as second
    doAsyncThing((error, result) => {})

Promises

    doAsyncThing()
    .then(result => {})
    .catch(error => {})

async / await

    const run = async () => {
    const results = await doAsyncThing() // must return a promise
    console.log('hello')
    }

### Error handling

Errors kill your app, just like the browser

Any thrown or unhandled errors will cause the process to crash and exit

Your app may have errors that should not cause a crash, so you must handle accordingly

### Server

A server's job is to handle a request from some sor of client (browser, mobile app, another server, etc)

one server, handling many requests from clients

Without considering scaling, one server instance will handle many client requests. Compared to a client app where that code only cared about itself on the host machine

Nodejs has built in and community packakes for build all types of servers (API's, static, realtime. etc)

### Debugging
Level 1

Use console.log to log your way through fixing your app. In production, record your logs

tldr; just like chrome

Level 2

Use the node inspector when starting your app and debug just like you would an browser app in chrome

``` node --inspect exercises/api/server.js ```
and we get debugger listening in websocket. It feeds the real data
hit `chrome://inspect` in chrome


Level 3

Text editor integration offers the most seamless experience.

### Testing

Earlier testing Dom(test browser app that use the dom in node) was handeled differently 
- karma starts headless version of browser(browser with no gui). It will start the browser up and inject all the test and talk via web sockets, run the test in the browser and give the result back.

- Now most framworks are virtualised now they can run on server
- Also we can mock the whole dom out using jsdom. and we can use the dom on the server


Before you can test your code, make sure it is testable. As long as you can export what you want to test, you should be able to test it. There are other concerns specific to what libraries and frameworks you use

 export your modules and use a testing framework

You can test pretty much anything in Nodejs. Browser apps, API's, CLI's, scripts, tools, etc. Your test themselves will be executed in Nodejs, so they have the ability to pretty much do anything.

### Anatomy of tests
 many tools for the same job
Your code to be tested
Test Suite - responsible for helping organize your tests, provide hooks, and overall environment
Assertion library - does the actual comparisons in your test
Mocks + Spies - tools to help you test your code without testing other code or actually running your code (mock out api calls, check to see if an internal function was called)

