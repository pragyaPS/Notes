Javascript was traditionally the language of the web browser, performing computations directly on a user's machine. This is referred to as 'client-side' processing. With the advent of Node.js, javasccript has become a compelling "server-side language as well, which was traditionally the domain of language like Java, Python and PHP.
In web development, an **isomorphic application** is one whose code (In this case JS) can run both in server and the client.

- React is an isomorphic javascript library built by facebook
- by inspecting a react application source code we can find that it loads bundle.js. and executing this bundle will take couple of seconds
-bundle contains the list of routes, templates, and components required to navigate through the website.

Here is an example of request processin 
- We hit the website url in the browser
- The request reached the web server and was passed to a Nodejs application.
- Node Js passed the request to React, which fetched the article's data db, built the full page,and return it to node js
- The user will see the response in HTML, while the browser downloads the react application(bundle.js) asynchronously

-Server side rendering 


