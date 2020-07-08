- export {}
- --watch
- let and const: block level scope. let can be declared but const has to be intilized else it will show error
- typescript gives static typechecking which can be unnoticed in plain javascript
- provides intelisence
- Array syntax
 let list1: number[] = [1,2,3];
 let list2: Array<number> [1,2,3]
- By default if  we initilize a variable without specifying type, typescript infurs the type for us.
- **union of type** of the same variable
    let multitype: boolean | number;
    multitype = 10;
    multitype = false; // both will not throw error
