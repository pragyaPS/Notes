https://kentcdodds.com/blog/unit-vs-integration-vs-e2e-tests

Effective snapshot testing
https://kentcdodds.com/blog/effective-snapshot-testing



jest uses node env so to access the dom for testing node uses JSDom to expose almost all Dom APIs


jsdom is a pure-JavaScript implementation of many web standards, notably the WHATWG DOM and HTML Standards, for use with Node.js. In general, the goal of the project is to emulate enough of a subset of a web browser to be useful for testing and scraping real-world web applications.

Component has two user 
1) Dev
2) End user

React testing library works the way native javascript code and we do not need to rememer the API function

dispatch event


End to End test and how its different

* unmount the component
* rerender

**End to End**: A helper robot that behaves like a user to click around the app and verify that it functions correctly. Sometimes called "functional testing" or e2e.
**Integration**: Verify that several units work together in harmony.
**Unit**: Verify that individual, isolated parts work as expected.
**Static**: Catch typos and type errors as you write the code.



