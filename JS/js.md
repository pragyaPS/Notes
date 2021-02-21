### Higher-order functions
- Takes in a function or passes out a function
- Just a term to describe these functions - any function that does it we call that - but
there's nothing different about them inherently

### Closures
good for performance in order to not repeat task or code
- Closure is the most esoteric of JavaScript concepts
- Enables powerful pro-level functions like ‘once’ and ‘memoize’
- Many JavaScript design patterns including the module pattern use closure
- Build iterators, handle partial application and maintain state in an
asynchronous world

### Functions with memories
- When our functions get called, we create a live store of data (local
memory/variable environment/state) for that function’s execution context.
- When the function finishes executing, its local memory is deleted (except the
returned value)
- But what if our functions could hold on to live data between executions?
- This would let our function definitions have an associated cache/persistent
memory
- But it all starts with us returning a function from another function