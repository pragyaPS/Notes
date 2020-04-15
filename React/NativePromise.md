

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

    var p = Promise.resolve( 21 );
    p
    .then( function(v){
        console.log( v );   // 21

        // fulfill the chained promise with value `42`
        return v * 2;
    } )
    // here's the chained promise
    .then( function(v){
        console.log( v );   // 42
    } );

Another example


    var p = Promise.resolve( 21 );
    p.then( function(v){
    console.log( v );   // 21
        return new Promise( function(resolve,reject){
            // introduce asynchrony!
            setTimeout( function(){
                // fulfill with value `42`
                resolve( v * 2 );
            }, 100 );
        } );
    } )
    .then( function(v){
        // runs after the 100ms delay in the previous step
        console.log( v );   // 42
    } );

If you don’t return an explicit value, an implicit undefined is assumed, and the promises still chain together the same way. Each Promise resolution is thus just a signal to proceed to the next step.


If you call then(..) on a promise, and you only pass a fulfillment handler to it, an assumed rejection handler is substituted:

    var p = new Promise( function(resolve,reject){
        reject( "Oops" );
    } );

    var p2 = p.then(
        function fulfilled(){
            // never gets here
        }
        // assumed rejection handler, if omitted or
        // any other non-function value
    passed
        // function(err) {
        //     throw err;
        // }
    );

Similarly, If a proper valid function is not passed  as the fulfillment handler parameter to then(..), there’s also a default handler substituted:

var p = Promise.resolve( 42 );

p.then(
    // assumed fulfillment handler, if omitted or
    // any other non-function value passed
    // function(v) {
    //     return v;
    // }
    null,
    function rejected(err){
        // never gets here
    }
);

The first callback parameter of the Promise(..) constructor will unwrap either a thenable (identically to Promise.resolve(..)) or a genuine Promise: 
    var rejectedPr = new Promise( function(resolve,reject){
        // resolve this promise with a rejected promise
        resolve( Promise.reject( "Oops" ) );
    } );

    rejectedPr.then(function fulfilled(){
            // never gets here
        },
        function rejected(err){
            console.log( err ); // "Oops"
        }
    );


### Error handeling

The most natural form of error handling for most developers is the synchronous try..catch construct. Unfortunately, it’s synchronous-only, so it fails to help in async code patterns:

    function foo() {
        setTimeout( function(){
            baz.bar();
        }, 100 );
    }

    try {
        foo();
        // later throws global error from `baz.bar()`
    }
    catch (err) {
        // never gets here
    }

In callbacks, some standards have emerged for patterned error handling, most notably the error-first callback style:

    function foo(cb) {
        setTimeout( function(){
            try {
                var x = baz.bar();
                cb( null, x ); // success!
            }
            catch (err) {
                cb( err );
            }
        }, 100 );
    }

    foo( function(err,val){
        if (err) {
            console.error( err ); // bummer :(
        }
        else {
            console.log( val );
        }
    } );

The try..catch here works only from the perspective that the baz.bar() call will either succeed or fail immediately, synchronously. If baz.bar() was itself its own async completing function, any async errors inside it would not be catchable.

This sort of error handling(first parameter, err.) is technically async capable, but it doesn’t compose well at all. Multiple levels of error-first callbacks woven together with these ubiquitous if statement checks will inevitably lead you to the perils of callback hell

Promises don’t use the popular error-first callback design style, but instead use split-callback style; there’s one callback for fulfillment and one for rejection.

    var p = Promise.resolve( 42 );

    p.then(
        function fulfilled(msg){
            // numbers don't have string functions,
            // so will throw an error
            console.log( msg.toLowerCase() );
        },
        function rejected(err){
            // never gets here
        }
    );

it’s because that error handler is for the p promise, which has already been fulfilled with value 42.The p promise is immutable, so the onlypromise that can be notified of the error is the one returned from **p.then(..)**, which in this case we don’t capture. Hence, It’s far too easy to have errors swallowed,

* If you use the Promise API in an invalid way and an error occurs that prevents proper Promise construction, the result will be an immediately thrown exception, not a rejected Promise. Some examples of incorrect usage that fail Promise construction: new Promise(null), Promise.all(), Promise.race(42), and so on. You can’t get a rejected Promise if you don’t use the Promise API validly enough to actually construct a Promise in the first place!


**To avoid losing an error to the silence of a forgotten/discarded Promise, some developers have claimed that a best practice for Promise chains is to always end your chain with a final catch(..),**

    var p = Promise.resolve( 42 );

    p.then(
        function fulfilled(msg){
            // numbers don't have string functions,
            // so will throw an error
            console.log( msg.toLowerCase() );
        }
    )
    .catch( handleErrors );


**What happens if handleErrors(..) itself also has an error in it? Who catches that?**

### Uncaught Handling

Some Promise libraries have added methods for registering something like a “global unhandled rejection” handler, which would be called instead of a globally thrown error. But their solution for how to identify an error as uncaught is to have an arbitrary-length timer, say three seconds, running from time of rejection.

Another more common suggestion is that Promises should have a **done(..)** added to them, which essentially marks the Promise chain as done.

So what happens instead? It’s treated as you might usually expect in uncaught error conditions: any exception inside a done(..) rejection handler would be thrown as a global uncaught error (in the developer console, basically):

    var p = Promise.resolve( 42 );

    p.then(
        function fulfilled(msg){
            // numbers don't have string functions,
            // so will throw an error
            console.log( msg.toLowerCase() );
        }
    )
    .done( null, handleErrors );

    // if `handleErrors(..)` caused its own exception, it would
    // be thrown globally here

But the biggest problem is that it’s not part of the ES6 standard, so no matter how good it sounds, at best it’s a lot longer way off from being a reliable and ubiquitous solution.

#### Pit of success(Theoretical)

* Promises could default to reporting (to the developer console) any rejection, on the next Job or event loop tick, if at that exact moment no error handler has been registered for the Promise.
* For the cases where you want a rejected Promise to hold onto its rejected state for an indefinite amount of time before observing, you could call defer(), which suppresses automatic error reporting on that Promise.you can implement this or use a smarter Promise library that does so for you!

### Promise Patterns

These patterns serve to simplify the expression of async flow control. Two such patterns are codified directly into the native ES6 Promise implementation,

 #### Promise.all([ .. ])
Asyn sequence(Promise chain), step2 depends on step1.
In classic programming terminology, a gate is a mechanism that waits on two or more parallel/concurrent tasks to complete before continuing. It doesn’t matter what order they finish in, just that all of them have to complete for the gate to open and let the flow control through. In the Promise API, we call this pattern all([ .. ]).

    // `request(..)` is a Promise-aware Ajax utility,
    // like we defined earlier in the chapter

    var p1 = request( "http://some.url.1/" );
    var p2 = request( "http://some.url.2/" );

    Promise.all( [p1,p2] )
    .then( function(msgs){
        // both `p1` and `p2` fulfill and pass in
        // their messages here
        return request(
            "http://some.url.3/?v=" + msgs.join(",")
        );
    } )
    .then( function(msg){
        console.log( msg );
    } );

**an array of all the fulfillment messages from the passed in promises, in the same order as specified (regardless of fulfillment order). **

* Technically, the array of values passed into Promise.all([ .. ]) can include Promises, thenables, or even immediate values. Each value in the list is essentially passed through Promise.resolve(..) to make sure it’s a genuine Promise to be waited on, so an immediate value will just be normalized into a Promise for that value. If the array is empty, the main Promise is immediately fulfilled.

If any one of those promises is instead rejected, the main Promise.all([ .. ]) promise is immediately rejected, discarding allresults from any other promises. Remember to always attach a rejection/error handler to every promise, even and especially the one that comes back from Promise.all([ .. ]).


#### Promise.race([ .. ])

While Promise.all([ .. ]) coordinates multiple Promises concurrently and assumes all are needed for fulfillment, sometimes you want to respond only to the “first Promise to cross the finish line,” letting the other Promises fall away. This pattern is classically called a latch, but in Promises it’s called a race.

unfortunately “race” is kind of a loaded term, because race conditions are generally taken as bugs in programs. Don’t confuse Promise.race([..]) with a race condition.

A race requires at least one “runner,” so if you pass an empty array, instead of immediately resolving, the main race([..]) Promise will never resolve. This is a footgun! ES6 should have specified that it either fulfills, rejects, or just throws some sort of synchronous error. Unfortunately, because of precedence in Promise libraries predating ES6 Promise, they had to leave this gotcha in there, so be careful never to send in an empty array.

    // `foo()` is a Promise-aware function

    // `timeoutPromise(..)`, defined ealier, returns
    // a Promise that rejects after a specified delay

    // setup a timeout for `foo()`
    Promise.race( [
        foo(),                  // attempt `foo()`
        timeoutPromise( 3000 )  // give it 3 seconds
    ] )
    .then(
        function(){
            // `foo(..)` fulfilled in
    time!
        },
        function(err){
            // either `foo()` rejected, or it just
            // didn't finish in time, so inspect
            // `err` to know which
        }
    );


#### Finally

The key question to ask is, “What happens to the promises that get discarded/ignored?” We’re not asking that question from the performance perspective — they would typically end up garbage collection eligible — but from the behavioral perspective (side effects, etc.). Promises cannot be canceled — and shouldn’t be as that would destroy the external immutability trust discussed in **“Promise Uncancelable”** — so they can only be silently ignored. But what if foo() in the previous example is reserving some sort of resource for usage, but the timeout fires first and causes that promise to be ignored? Is there anything in this pattern that proactively frees the reserved resource after the timeout, or otherwise cancels any side effects it may have had? What if all you wanted was to log the fact that foo() timed out? Some developers have proposed that Promises need a finally(..) callback registration, which is always called when a Promise resolves, and allows you to specify any cleanup that may be necessary. This doesn’t exist in the specification at the moment, but it may come in ES7+. We’ll have to wait and see. It might look like: 
    var p = Promise.resolve( 42 );

    p.then( something )
    .finally( cleanup )
    .then( another )
    .finally( cleanup );

Some library provides function for promise like any([ .. ]), first([ .. ]), last([ .. ])here’s how we could define first([ .. ]). 

    // polyfill-safe guard check
    if (!Promise.first) {
        Promise.first = function(prs) {
            return new Promise( function(resolve,reject){
                // loop through all promises
                prs.forEach( function(pr){
                    // normalize the value
                    Promise.resolve( pr )
                    // whichever one fulfills first wins, and
                    // gets to resolve the main promise
                    .then( resolve );
                } );
            } );
        };
    }

#### Concurrent iteration

Sometimes you want to iterate over a list of Promises and perform some task against all of them, much like you can do with synchronous arrays (e.g., forEach(..), map(..), some(..), and every(..)). If the task to perform against each Promise is fundamentally synchronous, these work fine, just as we used forEach(..) in the previous snippet. But if the tasks are fundamentally asynchronous, or can/should otherwisebe performed concurrently, you can use async versions of these utilities as provided by many libraries.

Consider an asynchronous map(..) utility that takes an array of values (could be Promises or anything else), plus a function (task) to perform against each. map(..) itself returns a promise whose fulfillment value is an array that holds (in the same mapping order) the async fulfillment value from each task:

    if (!Promise.map) {
        Promise.map = function(vals,cb) {
            // new promise that waits for all mapped promises
            return Promise.all(
                // note: regular array `map(..)`, turns
                // the array of values into an array of
    // promises
                vals.map( function(val){
                    // replace `val` with a new promise that
                    // resolves after `val` is async mapped
                    return new Promise( function(resolve){
                        cb( val, resolve );
                    } );
                } )
            );
        };
    }

Use of the function

    Promise.map( [p1,p2,p3], function(pr,done){
        // make sure the item itself is a Promise
        Promise.resolve( pr )
        .then(
            // extract value as `v`
            function(v){
                // map fulfillment `v` to new value
                done( v * 2 );
            },
            // or, map to promise
    rejection message
            done
        );
    } )
    .then( function(vals){
        console.log( vals );    [42,84,"Oops"]
    } );

#### Recap

* reject(..) simply rejects the promise, but resolve(..) can either fulfill the promise or reject it, depending on what it’s passed. If resolve(..) is passed an immediate, non-Promise, non-thenable value, then the promise is fulfilled with that value.
But if resolve(..) is passed a genuine Promise or thenable value, that value is unwrapped recursively, and whatever its final resolution/state is will be adopted by the promise.
 
* Promise.resolve(..) is usually used to create an already-fulfilled Promise in a similar way to Promise.reject(..). However, Promise.resolve(..) also unwraps thenable values. In that case, the Promise returned adopts the final resolution of the thenable you passed in, which could either be fulfillment or rejection. Promise.resolve(..) doesn’t do anything if what you pass is already a genuine Promise; it just returns the value directly. So there’s no overhead to calling Promise.resolve(..) on values that you don’t know the nature of, if one happens to already be a genuine Promise.

#### Promise Performance

it’s clear Promises have a fair bit more going on, which means they are naturally at least a tiny bit slower. Think back to just the simple list of trust guarantees that Promises offer, as compared to the ad hoc solution code you’d have to layer on top of callbacks to achieve the same protections. More work to do, more guards to protect, means that Promises are slower as compared to naked, untrustable callbacks.

Promises are a little slower, but in exchange you’re getting a lot of trustability, non-Zalgo predictability, and composability built in. Maybe the limitation is not actually their performance, but your lack of perception of their benefits?

They don’t get rid of callbacks, they just redirect the orchestration of those callbacks to a trustable intermediary mechanism that sits between us and another utility.




    






 







 
