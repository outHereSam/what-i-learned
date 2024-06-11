# Functions

### What is a function?

A function at its core is a block of code designed to perform a specific task.

### Basic structure of a function

```jsx
// Function declaration
function greet(name) {
  return "Hello " + name + "!";
}

// Calling the function
console.log(greet("Sam"));
```

Javascript functions can be defined in various ways. Some of these are;

- Function declaration
  This type of function is declared using the `function` keyword and can be called before they are defined because of _hoisting._

```jsx
greet("Sam"); // this will work just fine

// Function declaration
function greet(name) {
  return "Hello " + name + "!";
}
```

- Function expression
  This type of function is also declared using the `function` keyword but is given as a value in a binding;
  ```jsx
  // Function expression
  const greet = function (name) {
    return "Hello " + name + "!";
  };
  ```
  **_NB: These type of functions are not hoisted, therefore a call to the function before it’s declaration or definition would throw an error._**
- Arrow functions
  A shorter syntax for writing function expressions.
  ```jsx
  // Arrow function
  const greet = (name) => {
    return "Hello " + name + "!";
  };
  ```

### Parameters & Arguments

Parameters are variables that are listed as part of the function definition. Arguments are passed to the function as values for the parameters when the function is called.

```jsx
function add(a, b) {
  // a and b are parameters
  return a + b;
}

console.log(add(2, 3)); // 2 and 3 are arguments

// Default Parameters
function greet(name = "Guest") {
  return "Hello " + name + "!";
}

console.log(greet()); // Output will be "Hello, Guest!" since no argument was given
```

### Closure

A closure is a function that has access to it’s own scope, the scope of the function it’s within and the global scope

```jsx
function outerFunction(outerVariable) {
  return function innerFunction(innerVariable) {
    console.log("Outer variable: " + outerVariable);
    console.log("Inner variable: " + innerVariable);
  };
}

const newFunction = outerFunction("outside");
newFunction("inside");
// Output:
// Outer Variable: outside
// Inner Variable: inside
```

### Higher-Order Functions

Functions that take other function as arguments or return functions as their result.

```jsx
function add(a) {
  return function (b) {
    return a + b;
  };
}

const add5 = add(5);
console.log(add5(10)); // output: 15
```
