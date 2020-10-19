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





