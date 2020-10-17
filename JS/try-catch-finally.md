1. Return statement inside the try or catch block

If we have a finally block, the return statement inside try and catch block are not executed. It will always hit the finally block.
eg1:

    function test() {
    try {
    return 10;
    } finally {
    console.log("finally");
    return 1;
    }
    }
    console.log( test() ); // finally 1

Eg2:

    function test() {
    try {     
        return 10;
        throw "error"; // this is not executed, control goes to finally
    } catch {
        console.log("catch"); 
        return 1;
    } finally { 
        console.log("finally");
        return 1000;
    }
    }
    console.log( test() ); // finally 1000

2. Variables declared inside a try block are not available in the catch or finally block

If we use let or const to declare a variable in the try block, it will not be available to catch or finally. This is because these variable declarations are block-scoped.

    try {
    
        let a = 10;
        throw "a is block scoped ";
    } catch(e) {
        
        console.log("Reached catch");
        console.log(a); // Reference a is no defined
    }

But if we use var instead of let or const, then it will be available inside the catch because var is function scoped, and the declaration will be hoisted.

    try {
    
        var a = 10;
        throw "a is function scoped ";
    } catch(e) {
        
        console.log("Reached catch");
        console.log(a); // 10
    }

3. Catch without error details
In ES2019, the argument for the catch block is optional.

    try {
    // code with bug;
    } catch {
    // catch without exception argument
    }

4. try…catch will not work on setTimeOut

If an exception happens in “scheduled” code, like setTimeout, then try..catch won’t catch it.

    function callback() {
        // error code
    }
    function test() {
    try {
        setTimeout( callback , 1000);
    } catch (e) {
        console.log( "not executed" );
    }
    }