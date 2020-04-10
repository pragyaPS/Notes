

    function add(xPromise,yPromise) {
        // `Promise.all([ .. ])` takes an array of promises,
        // and returns a new promise that waits on them
        // all to finish
        return Promise.all( [xPromise, yPromise] )

        // when that promise is resolved, let's take the
        // received `X` and `Y` values and add them together.
    .then( function(values){
            // `values` is an array of the messages from the
            // previously resolved promises
            return values[0] + values[1];
        } );
    }

    // `fetchX()` and `fetchY()` return promises for
    // their respective values, which may be ready
    // now or later.
    add( fetchX(), fetchY() )

    // we get a promise back for the sum of those
    // two numbers.
    // now we chain-call `then(..)` to wait for the
    // resolution of that returned promise.
    .then( function(sum){
        console.log( sum ); // that was easier!
    } );


it’s possible that the resolution of a Promise is rejection instead of fulfillment. Unlike a fulfilled Promise, where the value is always programmatic, a rejection value — commonly called a rejection reason — can either be set directly by the program logic, or it can result implicitly from a runtime exception.

With Promises, the then(..) call can actually take two functions, the first for fulfillment (as shown earlier), and the second for rejection:

    add( fetchX(), fetchY() )
    .then(
        // fullfillment handler
        function(sum) {
            console.log( sum );
        },
        // rejection handler
        function(err)
    console.error( err ); // bummer!
        }
    );

once a Promise is resolved, it stays that way forever — it becomes an immutable value at that point — and can then be observed as many times as necessary.


Because a Promise is externally immutable once resolved, it’s now safe to pass that value around to any party and know that it cannot be modified accidentally or maliciously. This is especially true in relation to multiple parties observing the resolution of a Promise. It is not possible for one party to affect another party’s ability to observe Promise resolution. Immutability may sound like an academic topic, but it’s actually one of the most fundamental and important aspects of Promise design, and shouldn’t be casually passed over.

### Promises are an easily repeatable mechanism for encapsulating and composing future values.

Ways of handelling promise
 * foo is a function which returns promise that either resolve or reject

    var p = foo( 42 );
    bar( p );
    baz( p );

Another way to approach this 
    function bar() {
        // `foo(..)` has definitely finished, so
        // do `bar(..)`'s task
    }

    function oopsBar() {
        // oops, something went wrong in `foo(..)`,
        // so `bar(..)` didn't run
    }

    var p = foo( 42 );
    p.then( bar, oopsBar );
    p.then( baz, oopsBaz );


#### Thenable Duck Typing
    if (
    p !== null && (
            typeof p === "object" ||
            typeof p === "function") && typeof p.then === "function"
    ) {
        // assume it's a thenable!
    }
    else {
        // not a thenable
    }


    Object.prototype.then = function(){};
    Array.prototype.then = function(){};

    var v1 = { hello: "world" };
    var v2 = [ "Hello", "World" ];

Both v1 and v2 will be assumed to be thenables. You can’t control or predict if any other code accidentally or maliciously adds then(..) to Object.prototype, Array.prototype, or any of the other native prototypes.

 #### Calling Too Early
 
Promises by definition cannot be susceptible to this concern, because even an immediately fulfilled Promise (like new Promise(function(resolve){ resolve(42); })) cannot be observed synchronously.
No more need to insert your own setTimeout(..,0) hacks. Promises prevent Zalgo automatically.

    p.then( function(){
        p.then( function(){
            console.log( "C" );
        } );
        console.log( "A" );
    } );
    p.then( function(){
        console.log( "B" );
    } );  // ABC


jjh

    var p3 = new Promise( function(resolve,reject){
        resolve( "B" );
    } );

    var p1 = new Promise( function(resolve,reject){
        resolve( p3 );
    } );

    p2 = new Promise( function(resolve,reject){
        resolve( "A" );
    } );

    p1.then( function(v){
        console.log( v );
    } );

    p2.then( function(v){
        console.log( v );
    } );
// A B  <-- not  B A  as you might expect

see, p1 is resolved not with an immediate value, but with another promise p3, which is itself resolved with the value "B". The specified behavior is to unwrap p3 into p1, but asynchronously, so p1’s callback(s) are behind p2’s callback

To avoid such nuanced nightmares, you should never rely on anything about the ordering/scheduling of callbacks across Promises. In fact, a good practice is not to code in such a way where the ordering of multiple callbacks matters at all. Avoid that if you can.

### Never calling the callback

First, nothing (not even a JS error) can prevent a Promise from notifying you of its resolution (if it’s resolved). If you register both fulfillment and rejection callbacks for a Promise, and the Promise gets resolved, one of the two callbacks will always be called.

### The Promise.race() method returns a promise that fulfills or rejects as soon as one of the promises in an iterable fulfills or rejects, with the value or reason from that promise.

    const promise1 = new Promise(function(resolve, reject) {
        setTimeout(resolve, 500, 'one');
    });

    const promise2 = new Promise(function(resolve, reject) {
        setTimeout(resolve, 100, 'two');
    });

    Promise.race([promise1, promise2]).then(function(value) {
    console.log(value);
    // Both resolve, but promise2 is faster
    });
    // expected output: "two"

Something to be aware of: If you call resolve(..) or reject(..) with multiple parameters, all subsequent parameters beyond the first will be silently ignored. Although that might seem a violation of the guarantee we just described, it’s not exactly, because it constitutes an invalid usage of the Promise mechanism. Other invalid usages of the API (such as calling resolve(..) multiple times) are similarly protected, so the Promise behavior here is consistent (if not a tiny bit frustrating).

If at any point in the creation of a Promise, or in the observation of its resolution, a JS exception error occurs, such as a TypeError or ReferenceError, that exception will be caught, and it will force the Promise in question to become rejected.

    var p = new Promise( function(resolve,reject){
        foo.bar();  // `foo` is not defined, so error!
        resolve( 42 );  // never gets here :(
    } );

    p.then(
        function fulfilled(){
            // never gets here :(
        },
        function rejected(err){
            // `err` will be a `TypeError` exception object
            // from the `foo.bar()` line.
    }
    );

Hence, Promises turn even JS exceptions into asynchronous behavior, thereby reducing the race condition chances greatly.

If you pass an immediate, non-Promise, non-thenable value to Promise.resolve(..), you get a promise that’s fulfilled with that value. In this case, promises p1 and p2 will behave identically: 

    var p1 = new Promise( function(resolve,reject){
        resolve( 42 );
    } );
    var p2 = Promise.resolve( 42 );


But if you pass a genuine Promise to Promise.resolve(..), you just get the same promise back:

var p1 = Promise.resolve( 42 );
var p2 = Promise.resolve( p1 );

p1 === p2; // true


Even more importantly, if you pass a non-Promise thenable value to Promise.resolve(..), it will attempt to unwrap that value, and the unwrapping will keep going until a concrete final non-Promise-like value is extracted.

    var p = {
        then: function(cb) {
            cb( 42 );
        }
    };

    p.then(
        function fulfilled(val){
            console.log( val ); // 42
        },
        function rejected(err){
            // never gets here
        }
    );


So let’s say we’re calling a foo(..) utility and we’re not sure we can trust its return value to be a well-behaving Promise, but we know it’s at least a thenable. Promise.resolve(..) will give us a trustable Promise wrapper to chain off of: // don't just do this:
foo( 42 )
.then( function(v){
    console.log( v );
} );

// instead, do this:
Promise.resolve( foo( 42 ) )
.then( function(v){
    console.log( v );
} );


Another beneficial side effect of wrapping Promise.resolve(..) around any function’s return value (thenable or not) is that it’s an easy way to normalize that function call into a well-behaving async task. If foo(42) returns an immediate value sometimes, or a Promise other times,Promise.resolve( foo(42) ) makes sure it’s always a Promise result. And avoiding Zalgo makes for much better code.

two behaviors intrinsic to Promises:
*  Every time you call then(..) on a Promise, it creates and returns a new Promise, which we can chain with. 
*  Whatever value you return from the then(..) call’s fulfillment callback (the first parameter) is automatically set as the fulfillment of the chained Promise (from the first point).




 
