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

#### Snapshot testing
snapshot testing is an assertion, just like the toBe in: expect('foo').toBe('foo'). 

A test is code that throws an error when the actual result of something does not match the expected output.

it is relatively easy to test "pure functions" like those in our math.js module (functions which will always return the same output for a given input and not change the state of the world around them).

The part that says actual !== expected is called an "assertion." 
Node actually has an assert module for making assertions


    const assert = require('assert')
    const {sum, subtract} = require('./math')
    let result, expected
    result = sum(3, 7)
    expected = 10
    assert.strictEqual(result, expected)
    result = subtract(7, 3)
    expected = 4
    assert.strictEqual(result, expected)




