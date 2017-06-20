---
layout: lesson
title: "Map"
permalink: /map/
---

### Scope and Closure
- When you first start writing a program in JS, you are in the `Global Scope`. If we define a variable, it will be defined globally.
```javascript
  const number = 1;
```
We generally want to avoid globally scoped variables as you can very quickly run into namespacing issues.

- A local scope refers to any scope defined inside the global scope. Each function defined has its own local scope.
```javascript
  // scope 1: Global scope out here
  const myFunction = () => {
    // scope 2: Local scope in here
  };
```
Locally scoped items are not available to the global scope.

- When we define a function inside a function, the inner function has access to the scope of the outer function. But not the other way around. This is Closure! Thats all it is!!
```javascript
  // scope 1: Global scope out here
  const myFunction = () => {
    // scope 2
    const number = 1;
    const myOtherFunction = () => {
      // scope 3: `number` is accessible here!! We've created a closure because stuff defined in there isnt accessible outside of this function
    };
  };
```

## Anatomy of a function

```javascript
function multiplyNumberByTen(aNumber){
  return aNumber * 10;
}
```

- There are two big things we'll focus on here, the `arguments` and the `return` of a function.

#### Arguments

- The single argument is `aNumber`
- All Parameters are available within the () brackets of a function
- All arguments are also available in a variable only found in functions, called `arguments`
```javascript
function multiplyNumberByTen(aNumber){
  arguments[0]; // the same as aNumber 
}
```


#### Return

- Whatever is on the right hand side of a `return` statement is what the function resolves to

```js

function adder(num1, num2){
  return num1 + num2;
}

console.log(adder(3, 5)) // logs out 8 because it returns the result of 3 + 5;
```


```js

/** as a side note, this is still a pure function, as a pure function is defined by a function that always
returns the same value no matter what, as long as the inputs are the same, and has no internal dependencies that may change,
 as well as having no hidden 'outputs' - eg, it doesn't access an external object and mutate its value(s)
 **/
function multiplyNumberByTen(aNumber){
  return aNumber * 10;
}

let numberResult = multiplyNumberByTen(5);
```

- This is a well written function, and `numberResult` resolves to `50`;


### Interpreting our code
```javascript
let numberResult = multiplyNumberByTen(1) + multiplyNumberByTen(3) * multiplyNumberByTen(2);

function multiplyNumberByTen(aNumber){
  return aNumber * 10;
}
```
- functions can be used multiple times
- is seen by the interpreter like this
```javascript
let numberResult = 10 + 30 * 20;
```

- Remember order of operations, this resolves to `610` not `800`


```javascript
let numberResult = multiplyNumberByTen(multiplyNumberByTen(2) + 2);
function multiplyNumberByTen(aNumber){
  return aNumber * 10;
}
```

- to resolve is `multiplyNumberByTen(2) + 2`, we must resolve `multiplyNumberByTen(2)` first
- `numberResult = multiplyNumberByTen(20 + 2)`
- `numberResult = multiplyNumberByTen(22)`
- `numberResult = 220`


### Arrow Functions

```js
function adder(num1, num2) {
  return num1 + num2;
}
```

is equivalent to

```js
const adder = (num1, num2) => num1 + num2;
```

- Right hand side of the arrow is automatically returned

### Functions as 'callbacks'

- remember is that there is a difference between a function definition and a function being 'executed' or 'called'.

```javascript
let helloWorld = function(name){
  console.log(`Hello ${name}!`);
}

console.log(helloWorld);
```
- Logs out the function definition
- does **not** log out `Hello ${name}`

```javascript
function hollerBackFunc(callBackFunction){
  return callBackFunction(10);
}

const doubler = (num) => num * 2;
const addFive = (num) => num + 5;
const squarer = (num) => num * num;

const result1 = hollerBackFunc(doubler); // 20
const result2 = hollerBackFunc(addFive); // 15
const result3 = hollerBackFunc(squarer); // 100

```

- In the case of `hollerBackFunc`, the first argument it can expect is a function 
- This function, the parameter callBackFunction is then called **inside** `hollerBackfunc`, and 10 is always passed into whatever it gets
-The result if **that** is then returned. So let's take a look at that with `doubler`.
 
 ```javascript
const result1 = hollerBackFunc(doubler); //20
```
it ends up something like this
```javascript
function hollerBackFunc(/is now doubler*/){
  return (num => num * 2)(10);
}
```
- try copying `(num => num * 2)(10)` and pasting it in the console


### Functions that return functions

```js
const addWithDelay = firstNum => secondNum => firstNum + secondNum;

const delay = addWithDelay(5);
const final = delay(10); // final resolves to 15

// or can be written like this

const final = addWithDelay(5)(10); // note the 'double parenthesis'
```