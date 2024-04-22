# Mod 1 - Week 1 Study Guide 

**Table of Contents**
* [1-0 Intro to Node](#1-0-intro-to-node)
  * [What is Node?](#what-is-node)
  * [Modules](#modules)
  * [Node Package Manager](#node-package-manager)
  * [Scripts](#scripts)
  * [Jest & Testing](#jest--testing)
* [1-1 Variables, Functions, and Strings](#1-1-vars-functions-strings)
  * [Variables](#variables)
  * [Data Types](#data-types)
  * [Functions (Arrow Functions)](#functions-arrow-functions)
  * [Function Declarations](#function-declarations)
  * [Scope](#scope)
  * [String Properties and Methods](#string-properties-and-methods)
* [1-2 Control Flow](#1-2-control-flow)
  * [Conditional Statements](#conditional-statements)
  * [Guard Clauses](#guard-clauses)

## 1-0 Intro to Node

### What is Node?

* Two places to run JavaScript: **Node** and the **Browser**
* Run `.js` files using Node with the terminal command `node <filename.js>`

### Modules

* A **module** is a file that exports code to be reused in other files.
* A `.js` file can export values by assigning them to `module.exports`

```js
// math-utilities.js
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;
const SIMPLE_PI = 3.14;

// export these values in an Object
module.exports = {
  add, subtract, SIMPLE_PI
}
```

* Another `.js` file can import those values using the `require()` function and providing a relative path.

```js
// index.js

// destructure as "named" imports
const { add, subtract, SIMPLE_PI } = require('./math-utilities');

// or import the entire object as a "default" import
const mathUtils = require('./math-utilities');

console.log(add(5, 3)) // 8
console.log(mathUtils.add(5, 3)) // 8
```

* Often, we will destructure a module's exports immediately. This is called a "named export/import".

### Node Package Manager

* A **package** is a group of one or more modules published on the internet for use by other developers.
* **Node Package Manager** makes it easy to install and manage third-party packages.
* Packages published on NPM all have a `package.json` file that lists the package's **dependencies** and meta data.
* A **dependency** is a package relied upon by another package.

```sh
npm init          # creates a package.json file
npm i prompt-sync # installs prompt-sync as a dependency
npm i -D jest     # installs Jest as a devDependency
npm i -g nodemon  # installs nodemon as a global dependency
```

* When importing modules installed from NPM, `require()` only needs the package name (not a relative path)

```js
// npm module import
const prompt = require('prompt');

// local module import
const mathUtils = require('./math-utilities');
```

### Scripts

* **NPM Scripts** are defined in a package's `package.json` file and help you quickly execute a terminal command

```json
"scripts": {
  "preinstall": "cp hooks/pre-commit .git/hooks/pre-commit",
  "play": "node playground.js",
  "start": "node index.js",
  "lint": "eslint .",
  "test": "jest --coverage",
  "test:w": "jest --watchAll"
},
```

* To run a script, enter `npm run <script name>`
  * Example: `npm run lint` or `npm run play`
* Some designated scripts can omit the "run" argument, such as:
  * `npm start`
  * `npm test`

## Jest & Testing

* **Jest** is a testing framework for JavaScript. 
* All testing files end in `.spec.js`. 
* A **test** in Jest is made by invoking `it()` with a test name and callback. The callback has `expect()` statements that all must be true for the test to pass.

```js
it('exports a named function called `add` from math-utilities.js', () => {
  const { add } = require('./math-utilities');
  expect(typeof add).toEqual('function');
});

it('returns the sum of two input numbers', () => {
  const { add } = require('./math-utilities');
  expect(add(5, 3)).toEqual(8);
  expect(add(1, 2)).toEqual(3);
  expect(add(100, 200)).toEqual(300);
});
```

* Every test should test one piece of functionality, though it may require multiple `expect()` statements to do so.
* A collection of tests is called a **test suite**

* Run `npm test` to see the results of a test file. Below is a test suite with all tests passing


<image src='https://raw.githubusercontent.com/The-Marcy-Lab-School/2023-Fall-Curriculum/a0ae173ad6275f2dc503a7ecfc722e3cb12ebeb6/mod-1/study-guides/img/tests-passing.png' style="border: 1px solid black;">

* A failing test will show the `expect()` statement that failed as well as a comparison of what it "expected" vs. what it "received":

<image src='https://raw.githubusercontent.com/The-Marcy-Lab-School/2023-Fall-Curriculum/a0ae173ad6275f2dc503a7ecfc722e3cb12ebeb6/mod-1/study-guides/img/tests-failing.png' style="border: 1px solid black;">


## 1-1 Vars, Functions, Strings

### Variables

- A **variable** "binds" a piece of data in memory to an "identifier" (the variable's name)
- Use `let` or `const` to declare a new variable (don't use `var`)

```js
let age = 18;     // let declares a reassignable variable
const PI = 3.14;  // const declares a non-reassignable variable

age = 19;       // Allowed
PI = 3.14159;   // TypeError: cannot reassign const variables
```

- Variables must be referenced AFTER they are declared. Otherwise, a `ReferenceError` will be thrown.
- Variables declared with `var` can be referenced BEFORE they are declared but will have the value `undefined`. This is due to **hoisting** and should be avoided.

**Q: You are writing a program that involves a variable called `clickCount` that will keep track of how many times a user clicks on a button. Should `clickCount` be declared with `let` or `const`?**

<details><summary>Answer</summary>

`clickCount` should be declared with the `let` keyword because we can assume that it will be reassigned each time the user clicks on the button. 

</details>

### Data Types:

- Variables can store data of any kind **data type**: String, Number, Boolean, Undefined, Null, Function, or Object.
- Primitive Data Types are String, Number, Boolean, `undefined` and `null`. 

```js
// Strings are text wrapped in 'single quotes', "double quotes",or `backticks`
const phrase = "Hello World!";
const name = 'Madhur';
const message = `My name is ${name}. I say ${phrase}`; // backticks allow for "String Interpolation"

// Numbers can be positive, negative, or include a decimal point
const PI = 3.14159;
const age = 18;
const countdown = -3;

// Booleans are either `true` or `false`
const canDrive = true;
const canFly = false;

// Null is a non-value that is explicitly set by the programmer
let toBeDetermined = null;

// Undefined is a non-value that is automatically set for variables that are not assigned a value:
let emptyVariable; 
```

**Q: You are building a social media app where users have an email, a follower count, and an `isAdmin` status. Which data type would you use to represent each of these values?**

<details><summary>Answer</summary>

- email: String
- follower count: Number
- `isAdmin`: Boolean
  
</details>

**Q: What are the similarities and differences between `null` and `undefined`?**

<details><summary>Answer</summary>

`null` and `undefined` both represent non-values but `null` must be explicitly assigned while `undefined` is automatically assigned to variables/parameters without a value.

</details>

### Functions (Arrow Functions)

- A **function** is a block of code bound to a variable. 
  - An **arrow function** is written `(parameter) => { code block }` and is stored in a variable.
- After a function is declared, it can be **invoked** by ptting `()` after the function's name. Doing so causes the function's code block to execute.

```js
const add = (a, b) => { // arrow function definition
  const sum = a + b;
  return sum;
}
const sum1 = add(5,3); // function invocation
const sum2 = add(1,2);
```

- A **parameter** is a variable serving as a placeholder for data passed into a function. 
- An **argument** is a value "passed" to a function during a function invocation. 
- One argument must be provided for each parameter in the function definition.
- When a function is invoked, the invocation will evaluate to a value determined by the function's `return` statement (or it will return `undefined` if not specified).

**Q: What will be printed to the console after executing this program? What value will `result` hold?**

```js
const greet = (name) => {
  const message = `Hello ${name}, how are you?`;
  console.log(message);
}

const result = greet('Madhur');
```

<details><summary>Answer</summary>

`result` will hold the value `undefined` because `greet` doesn't have a `return` statement. As a result, the function will return `undefined` by default when it is invoked. 

As a side-effect, the invocation of `greet('Madhur')` will print `"Hello Madhur, how are you?"` to the console.

</details>

### Function Declarations

* A function can also be created using the **function declaration** syntax. It can be identified by the use of the `function` keyword.

```js
greet(); // this weirdly works
function greet(name) {
  console.log(`Hello ${name}, how are you?`);
}
```

* Functions created using this syntax are **hoisted** (like variables declared with `var`) and are thus invocable _before_ their declaration appears in the file.
* For consistency, this should be avoided

### Scope

* Variables declared within the a code block with `let` and `const` are only "visible" within that code block, including nested code blocks.
* The following JavaScript constructs use code blocks that create their own **scope**:
  * Functions
  * If Statements
  * Loops

```js
let a = "A globally scoped variable"
const myScopedFunction = () => {
  let b = "I can be seen ANYWHERE in this function";
  if (true) {
    let c = "I'm only visible inside this if statement";
    console.log(a, b, c); // We can reference all 3 values here
  }
  console.log(a, b); // but we can't reference c here
}
console.log(a); // and we can only reference a here
```
* Variables declared with `var` are only scoped to functions, not `if` statements or loops. Another reason to avoid using `var`.

```js
if (true) {
  var a = 'hi';
}
console.log(a); // this works
```

* Variables declared without any keyword are globally scoped, regardless of where they are declared. This should always be avoided.

```js
const foo = () => {
  x = 5; // a global scoped variable. Don't do this.
}
```

**Q: Why is scope important?**

<details><summary>Answer</summary>

1. Scope helps reduce memory usage by effectively "throwing away" a variable once we leave the scope of that variable. 
2. Scope can also reduce variable name conflicts. We will often use common variable names for repeated tasks, for example: declaring the variable `i` for a `for` loop. Without scope, every `for` loop would need to use a different variable name to avoid conflicts.

</details>

### String Properties and Methods

* A string is a series of characters wrapped in quotations ('', "", or ``)
* Each character in a string has an **index** - a number indicating the character's position in the string, starting at `0`.
* **Bracket Notation** can be used to read the character at the given index:

```js
console.log("Hello");     // Hello
console.log("Hello"[0]);  // H
console.log("Hello"[4]);  // o
console.log("Hello"[5]);  // undefined
```

* Every string has a `.length` property which returns the number of characters in the string:

```js
console.log("Hello".length);  // 5
const name = 'Madhur';
const lastChar = name[name.length - 1];
```

* Every string has **methods** for manipulating the string. A method is a function that is invoked direclty "on" a data value:

```js
console.log("Hello".indexOf("H"));      // 0
console.log("Hello".indexOf("o"));      // 4
console.log("Hello".includes("ell"));   // true
console.log("Hello".includes("h"));     // false
console.log("Hello".toUpperCase());     // HELLO
console.log("Hello".toLowerCase());     // hello
console.log("Hello".slice(1, 3));       // el 
console.log("Hello".slice(1));          // ello
console.log("Hello".slice());           // Hello
```

* These are some of the most common String methods.

**Q: Write a function called `startsWith` that takes in a string and a character and returns `true` if the string starts with that character.**

<details><summary>Answer</summary>

```js
const startsWith = (str, char) => {
  return str[0] === char;
  // or 
  // return str.startsWith(char);
}
```

</details>

**Q: Write a function called `capitalize` that takes in a string and returns a copy of that string with the first letter capitalized and the rest in lowercase.**

<details><summary>Answer</summary>

```js
const capitalize = (str) => {
  return str[0].toUpperCase() + str.slice(1).toLowerCase();
}
```

</details>

## 1-2 Control Flow

### Conditional Statements

* A **conditional statement** is a block of code that will or will not run depending on the outcome of a **Boolean expression** (an expression that resolves to `true` or `false`)
* Boolean Expressions can be created using **comparison operators**: `< <= > >= === !==`
* A conditional statement begins with an `if` statement, can be followed by 0 or more `else if` statements, and can conclude with an `else` statement.

```js
const greet = (name, tone) => {
  if (tone === 'happy') {
    console.log(`Hey ${name}, it is great to see you!`);
  } else if (tone === 'grumpy') {
    console.log(`Ah, it's you again... ${name}...`);
  } else if (tone === 'country') {
    console.log(`Howdy ${name}!`);
  } else {
    console.log(`Hi ${name}.`);
  }
}
greet('Madhur', 'happy');    // Hey Madhur, it is great to see you!
greet('Madhur', 'grumpy');   // Ah, it's you again... Madhur...
greet('Madhur', 'country');  // Howdy Madhur!
greet('Madhur');             // Hi Madhur.
```

* Only the first statement in a chain of `if`/`else if`/`else` statements whose condition evaluates to `true` will be executed. 
* An `else` statement is only executed if all the prior `if` and `else` if statements all evaluate to `false`.
* **Truthy** and **Falsey** values are non-Boolean values that will evaluate to `true` / `false` when placed in an `if` statement. 
* **Falsey** values are "empty" or "blank" or "non values": 
  * An empty string `""`
  * The number `0`
  * The number `NaN`
  * `undefined`
  * `null`
* All other values are truthy.

**Q: Write the classic `fizzBuzz` function! It should accept a number and log to the console `"fizz"` if the number is divisible by `3`, `"buzz"` if divisible by `5`, and `"fizzbuzz"` if divisible by both. If not divisible by any of these, print the input number itself. Do not return anything.**

<details><summary>Answer</summary>

```js
const fizzBuzz = (num) => {
  if (num % 3 === 0 && num % 5 === 0) {
    console.log("fizzbuzz");
  } else if (num % 3 === 0) {
    console.log("fizz");
  } else if (num % 5 === 0) {
    console.log("buzz");
  } else {
    console.log(num);
  }
}
```

We can also take advantage of the "falsey-ness" of the number `0` to rewrite the conditions in this function:

```js
const fizzBuzz = (num) => {
  if (!(num % 3) && !(num % 5)) {
    console.log("fizzbuzz");
  } else if (!(num % 3)) {
    console.log("fizz");
  } else if (!(num % 5)) {
    console.log("buzz");
  } else {
    console.log(num);
  }
}
```

If the remainder is `0`, then we flip its truthyness using `!`.

</details>


### Guard Clauses

* A **guard clause** is an `if` statement used at the start of a function that returns immediately. 
* Guard clauses are used to "fail fast" — if the current state (data) does not allow the rest of the function to operate properly, we exit early.
* Consider this example:

```js
const add = (a, b) => {
  if (typeof a !== 'number' || typeof b !== 'number') { // guard clause
    return NaN;
  } else { // do we need this else statement?
    return a + b;
  }
}
```

* In this example, we want to avoid adding non-number values. Strings, for example, will concatenate with the `+` operator, which we'd like to avoid.
* We can **refactor** (rewrite) this function without the `else` statement:

```js
const add = (a, b) => {
  if (typeof a !== 'number' || typeof b !== 'number') {
    return NaN;
  }
  return a + b;
}
```

* If the guard clause's `return` statement is executed, the function will exit. Therefore, we are not at risk of accidentally executing `return a + b`. 

**Q: Write a function called `getType` that takes in a value and returns the type of that value as a string, including the types `"NaN"`, `"Array"`, and  `"null"`**

<details><summary>Answer</summary>

```js
const getType = (value) => {
  if (Array.isArray(value)) {
    return "array";
  } 
  if (Number.isNaN(value)) {
    return "NaN";
  } 
  if (value === null) {
    return "null";
  } 
  return typeof value;
}
```

Rather than using a chain of `else if` statements, this solution uses a series of **guard clauses**.

</details>