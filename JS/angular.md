The architecture of an Angular application relies on certain fundamental concepts. The basic building blocks of the Angular framework are Angular components that are organized into NgModules. NgModules collect related code into functional sets; an Angular app is defined by a set of NgModules. An app always has at least a root module that enables bootstrapping, and typically has many more feature modules.

 - Components define views, which are sets of screen elements that Angular can choose among and modify according to your program logic and data.
 - Components use services, which provide specific functionality not directly related to views. Service providers can be injected into components as dependencies, making your code modular, reusable, and efficient.

 ### decorators
 In its simplest form, a decorator is simply a way of wrapping one piece of code with another  literally “decorating” it. This is a concept you might well have heard of previously as functional composition, or higher-order functions.

    function doSomething(name) {
    console.log('Hello, ' + name);
    }

    function loggingDecorator(wrapped) {
    return function() {
        console.log('Starting');
        const result = wrapped.apply(this, arguments); // this: window; arguments: ["Pragya"]
        console.log('Finished');
        return result;
    }
    }

    const wrapped = loggingDecorator(doSomething);
    wrapped("Pragya");



    function log(name) {
    return function decorator(t, n, descriptor) {
        const original = descriptor.value;
        if (typeof original === 'function') {
        descriptor.value = function(...args) {
            console.log(`Arguments for ${name}: ${args}`);
            try {
            const result = original.apply(this, args);
            console.log(`Result from ${name}: ${result}`);
            return result;
            } catch (e) {
            console.log(`Error from ${name}: ${e}`);
            throw e;
            }
        }
        }
        return descriptor;
    };
    }


    class Example {
    @log('some tag')
    sum(a, b) {
        return a + b;
    }
    }

    const e = new Example();
    e.sum(1, 2);
    // Arguments for some tag: 1,2
    // Result from some tag: 3
