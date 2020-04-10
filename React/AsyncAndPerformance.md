

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


Writing a function to add