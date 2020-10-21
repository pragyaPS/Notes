integer and decimal not diffrentiated in JS
3.0 === 3 // true

typeof: Unary operator that tells what type of the things
typeof null is "object" // bug in JS
typeof [1,2,3] // subtype of object not a bug
typeof undefine

### loops
    for(let i=0, i< students.length; i++>) {
        greetStudents(students[i]);
    }
    for(let student of students) {
        greetStudents(student)
    }
    while(student.length>0) {
        let 
    }

`` : interpolated string
.includes(str) : // works on string and check if str present as substring

### Three pillers of javascript
- types & coercion
- Scope/closures
- this/ Prototypes

### Types & coercion

- primitive types
- Converting Types
- Checking equality

types
- undefined
- number
- string
- boolean
- object // function and arrays are subtype

typeof null is object

NaN: invalid numeric expression

### new
- Object()
- Array()
- Function()
- Date()
- RegExp()
- Error()

### converting type:(coercion)
False values: 
0, -0
null
NaN
false
undefined

- == allows coercion (type different)
- === disallows coercion (type same)
In other words == & === are same if type is same. We should not completly discard the use of ==, instead make the type more predictable

    undefined == null
    true
    undefined === null
    false

    if(testval === null || testval === undefined)
    if(testval == null) // this checks for both null and undefined also more readable


### scope/ Closures

How the variables being picked and accessed
// defining a variable without declaring creates the var in global scope in non strict mode 

- closure is when a function 'remembers' the variables outside of it, even if you pass that function elsewhere

fuction ask(question) {
    setTimeout(() => {
        console.log(question) // 
    }, 100)
}


It remembers the variable even if the funcction executed in entirely different place or entirely different timeline

    function ask(question) {
        return function(){
            console.log(question)
        }
    }

    var myQuestion = ask("What is closure");
    myQuestion() // rememebers the question variable even if the execution finished

### this/prototype/class

A function's this refrences the execution context for that call. determine entirely by **how the function is being called**.

this: dynamic context

var workshop = {
    teacher: "Pragya",
    ask(question) {
        console.log(this.teacher)
    }
}

workshop.ask('this concept');
// Its based on how the function is being callehere we are calling ask function using workshop.ask so this will refre to workshop

    function ask(question) {
        console.log(this.teacher, question);
    }
    function otherClass() {
        var myContext = {
            teacher: "Suzy"
        };
        ask.call(myContext, "why");
    }
    otherClass();


### Prototypes

    function Workshop(teacher) {
        this.teacher = teacher;
    }
    Workshop.prototype.ask = function(question) {
        console.log(this.teacher, question);
    };

    var deepJS = new Workshop("Pragya");
    var reactJs = new Workshop("Singh");

    deepJS.ask("initilised with Pragya");
    reactJs.ask("Iitilised with Singh");



### Class

under the hood it does uses Prototype

    class Workshop {
        constructor(teacher) {
            this.teacher = teacher
        }
        ask(question) {
            console.log(question);
        }
    }

    var deepJS = new Workshop("Pragya");
    var reactJs = new Workshop("Singh");

    deepJS.ask("initilised with Pragya");
    reactJs.ask("Initilised with Singh");

------------------------------------------------
attach css using link
`<link rel="stylesheet" href="styles.css">`

### splice
let arrDeletedItems = array.splice(start[, deleteCount[, item1[, item2[, ...]]]])

let myFish = ['angel', 'clown', 'mandarin', 'sturgeon']
let removed = myFish.splice(2, 0, 'drum')

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice


### slice
The slice() method returns the selected elements in an array, **as a new array object**.

The slice() method selects the elements starting at the given start argument, and ends at, but does not include, the given end argument.

    var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
    var citrus = fruits.slice(1, 3);
    // Orange,Lemon

    var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
    var myBest = fruits.slice(-3, -1);
    // Lemon,Apple


- Set class takes an array and returns an object distinct values in the array

    let arr = [1,2,2,3,4,4];
    [...new Set(arr.map(item => item))] // return new array with distict item

- get SeconLargestNumber

    function getSecondLargest(nums) {
        let distictArray = [...new Set(nums.map(item => item))];
        let n = distictArray.length;
        let secondLargestNumber;
        if(distictArray.length == 1){
            secondLargestNumber = distictArray[0]
        } 
        else {
        secondLargestNumber = distictArray.sort()[n-2];
        }
        return secondLargestNumber;
        // Complete the function
    }


- Array.sort sorts the string 
[1,2,10].sort() // returns [1, 10, 2]
to sort the number pass the comapare function as callback

    [1,2,10].sort((a,b) => a-b); // returns [1,2,10]









