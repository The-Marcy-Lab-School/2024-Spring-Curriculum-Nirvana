# Mod 1 - Week 1 Study Guide 

**Table of Contents**
* [1-2-0 Callbacks & Higher Order Functions](#1-2-0-callbacks--higher-order-functions)
  * [First, A Normal Function](#first-a-normal-function)
  * [Callbacks & Higher Order Functions](#callbacks--higher-order-functions)
* [1-2-1 Array Higher Order Methods](#1-2-1-array-higher-order-methods)
  * [Declarative vs. Imperative Code](#declarative-vs-imperative-code)
  * [Array Higher Order Methods](#array-higher-order-methods)
* [1-2-2 Regex](#1-2-2-regex)
  * [Regex Basics](#regex-basics)
  * [Character Sets](#character-sets)
  * [Special Character Sets](#special-character-sets)
  * [Quantifiers](#quantifiers)
  * [Anchors](#anchors)
  * [Creating Regular Expressions in JavaScript](#creating-regular-expressions-in-javascript)
  * [Testing Patterns in JavaScript](#testing-patterns-in-javascript)
  * [Finding Matches](#finding-matches)


## 1-2-0 Callbacks & Higher Order Functions

### First, A Normal Function

```js
const greet = (person, msg, volumeLevel) => {
  let result = '';
  if (volumeLevel === 'loud') {
    result = `${person} said, "${msg.toUpperCase()}!"`
  } else if (volumeLevel === 'quiet') {
    result = `${person} said, "...${msg.toLowerCase()}..."`
  }
  console.log(result)
  return result;
}

greet('Maya', 'Hello', 'loud')
greet('Zo', 'Bye', 'quiet')
```

**Q: What are the limitations of this function?**

<details><summary>Answer</summary>

- There is repetitive code (the `if` statements)
- Each option must be hard-coded
- Has a little too much control over HOW the greeting is formatted

</details>


### Callbacks & Higher Order Functions

* A **callback** is a function that is given to another function as an argument
* A **higher order function (HOF)** is a function that takes in and/or returns a function
* HOFs invoke callbacks
* "We" do not invoke callbacks

```js
// These are callbacks
const yell = (msg) => `${msg.toUpperCase()}!!`;
const whisper = (msg) => `...${msg.toLowerCase()}...`;

// This is a "Higher-Order" function
const greetBetter = (person, msg, voiceCallback) => {
  const result = `${person} said, "${voiceCallback(msg)}"`
  console.log(result);
}

greetBetter('Maya', 'Hello', yell); // Maya said, HELLO!!
greetBetter('Zo', 'Bye', whisper);  // Zo said, ...bye...

/* we can use "inline arrow functions" */
greetBetter('Ben', 'Hello World', (msg) => `${msg}?`) // Ben said, Hello World?
```

* When we refactor this function to use callbacks we:
  - aren't repeating ourself
  - no longer need to specify HOW the person is greeting
  - aren't limited by the options we hard-coded
* **Inline arrow functions** allow us to define single-use callbacks without storing them in a variable first.



## 1-2-1 Array Higher Order Methods

### Declarative vs. Imperative Code

* **Imperative code** provides explicit instructions for exactly HOW to complete a task.
    * High control (you write every single line)
    * High effort (you write every single line)

```js
const doubleAllNums = (arr) => {
  const newArr = [];
  for (let i = 0; i < arr.length; i++) {
    const result = arr[i] * 2
    newArr.push(result)
  }
  return newArr;
}
const doubledNumsOld = doubleAllNums([1, 5, 10, 20]);
```

* **Declarative code** provides the desired solution without specifying HOW to get there.
    * Low control
    * Low effort

```js
const doubledNums = [1, 5, 10, 20].map((num) => num * 2);
```

### Array Higher Order Methods

* All Arrays have built-in higher-order methods for quickly iterating through the contents of the Array.
* The important ones to know are:

| Name                                  | When To Use                                      |
|---------------------------------------|--------------------------------------------------|
| `.forEach(callback)`                  | To iterate but not create a new Array            |
| `.map(modify)`                        | To make a copy of the Array with modified values |
| `.filter(test)`                       | To make a copy of the Array with filtered values |
| `.find(test)`                         | To get a single value from an Array              |
| `.findIndex(test)`                    | To get the index of a single value from an Array |
| `.reduce(accumulator, startingValue)` | To derive a single value from an Arary           |
| `.sort(compare)`                      | To sort the contents of an Array in place        |

* Invoke these directly ON the Array
* These are Higher Order **Methods** because they are functions stored within an Array "Object", not as a stand-alone function.

```js
const nums = [1,2,3,4,5];
const doubleNums = nums.map((num) => num * 2);
const evens = nums.filter((num) => num % 2 === 0);
const firstEven = nums.find((num) => num % 2 === 0);
```


* Each Higher Order Method takes in a callback function
* The first parameter of the callback should be named a singular version of the array name (`num` for an array of `nums`, `letter` for an array of `letters`, etc...)

**Examples**:

```js
const letters = ['a', 'b', 'c'];
const names = ['wendy', 'jon', 'daniel'];
const nums = [1, 3, 5, 7, 9];

// Generic Callback (callback doesn't return anything)
nums.forEach((num) => console.log(num));
nums.forEach((num, i, arr) => console.log(num, i, arr));
nums.forEach(console.log);

// Modify Callback (callback returns a new version of the value)
const doubled = nums.map((num) => num * 2);
const capsLetters = letters.map((letter) => letter.toUpperCase());
const firstLetter = names.map((name) => name[0]);

// Test Callback (callback tests the value, returns true/false)
const lessThanSeven = nums.filter((num) => num < 7); // get All values that pass the test
const seven = nums.find((num) => num < 7); // get the first value that passes the test
const indexOfSeven = nums.findIndex((num) => num === 7); // get the index of the first value that passes the test
const areAllOdd = nums.every((num) => num % 2 === 1); // do all the values pass the test?
const hasSomeMultiplesOfThree = nums.some((num) => num % 3 === 0); // does at least one value pass the test?

// Accumulator Callback (callback returns the next value of the accumulator)
const sumOfNums = nums.reduce((total, num) => total + num, 0);
```

**Q: You are given an Array of `letters`. You need to make a copy of the Array but with all the letters capitalized. What Array higher order method do you use? How would you use it?**

<details><summary>Answer</summary>

```js
letters.map((letter) => letter.toUpperCase())
```

</details><br>

**Q: You are given an Array of `numbers`. You need to make a copy of the Array but with only values less than `5`. What Array higher order method do you use? How would you use it?**

<details><summary>Answer</summary>

```js
numbers.filter((num) => num < 5);
```

</details><br>

**Q: You are given an Array of `numbers`. You need the sum of those numbers. What Array higher order method do you use? How would you use it?**

<details><summary>Answer</summary>

```js
numbers.reduce((total, num) => total + num, 0);
```

</details>

## 1-2-2 Regex

### Regex Basics

* **Regex** is short for **Regular Expression**
* A regular expression is a sequence of characters (or **tokens**) that specifies a **match pattern** in text.

![](img/regular_expression_example.png)

> Blue highlights show the match results of the regular expression pattern: `/h[aeiou]+/g` (the letter *h* followed by one or more vowels)

* A regular expression is written between a pair of forward slashes `/ /` 

* The letter `g` following the second `/` is a **flag** which alters the behavior of the regular expression. Important flags include:
  * `g` - the global flag matches ALL matches, not just the first one.
  * `i` - the case-insensitive flag disregards upper/lowercase when searching for matches
* Use https://regexr.com/ to test out your regular expressions!

### Character Sets

* **Character Sets** (or Character Class) are a grouping of characters, any of which may be included in a matching pattern.
* They are written inside of square brackets`[]`

![match all instances of b followed by a vowel followed by a t](./img/character-sets.png)

> Match all instances of *b* followed by a vowel followed by a *t*

* Characters sets can include **ranges** of letters/numbers too.
    * `[0-9]` - match any digit between 0 and 9
    * `[2-7]` - match any digit between 2 and 7 (inclusive)
    * `[a-g]` - match any lowercase letter between *a* and *g*
    * `[a-zA-Z]` - match any lowercase or uppercase letter

![Match all instances of the numbers 1-9 (not including 0](./img/character-set-range.png)

> Match all instances of the numbers 1-9 (not including 0)

* To match anything NOT in a character set, put `^` inside of the `[]` at the start:

![Match all instances of characters that are NOT a digit 1-9](./img/character-set-negated.png)
> Match all instances of characters that are NOT a digit 1-9

### Special Character Sets

* Common character sets (like all digits or all "word" characters) are represented with special characters/tokens
* They all begin with `\` and are followed by a single letter
* There is often an inverse version of each
  * `\d` - Match any digit (0-9)
  * `\D` - Match any NON digit 
  * `\w` - Match any "word" character (alpha-numeric or _)
  * `\W` - Match any NON-word character
  * `\s` - Match any whitespace character (spaces, tabs, line breaks);
  * `\S` - Match any NON-whitespace character
  * `\b` - Match any word-boundary *position* between a word character and a non-word character
  * `\B` - Match any NON-word-boundary
  * `.` - Matches any character except line breaks
  * `\.` - Matches a "." character

![Match all sequences of two word characters. Notice that punctuation and spaces are not included.](./img/word-class.png)

> Match all sequences of two word characters. Notice that punctuation and spaces are not included.

### Quantifiers

* Quantifiers allow us to specify the quantity of a particular character/character set
  * `?` - 0 or 1 of the preceding token
  * `*` - 0 or more of the preceding token
  * `+` - 1 or more of the preceding token
  * `{3}` - 3 of the preceding token (can be any number)
  * `{3,}` - 3 or more of the preceding token
  * `{3,5}` - 3-5 of the preceding token

![Match all sequences of 1 or more non-vowels](./img/quantifiers.png)

> Match all sequences of 1 or more non-vowels

### Anchors

* **Anchors** match the beginning and/or end of a string. 
  * `^` matches the beginning of the string, or the beginning of a line if the multiline flag (`m`) is enabled. This matches a position, not a character.
  * `$` matches the end of the string

![Matches the beginning of any line that starts with a vowel (case insensitive) and is followed by any number of word characters.](./img/anchor.png)

> Matches the beginning of any line that starts with a vowel (case insensitive) and is followed by any number of word characters.

### Creating Regular Expressions in JavaScript

* A regular expression is a type of Object and can be stored in a variable:
* Create a regular expression using the `RegExp` constructor:

```js
const regEx = new RegExp('ab', 'g');
const greet = new RegExp(`Hi ${name}`, 'g');
console.log(typeof regEx); // 'object'
```
* This is a bit clunky but you can make dynamic regular expressions.
* When using the `new RegExp` syntax, you must escape all `\`:

```js
const brokenRegEx = new RegExp(`\w\d`, 'g');
console.log(brokenRegEx);
// /wd/g - the \ are ignored

const goodRegEx = new RegExp(`\\w\\d`, 'g');
console.log(goodRegEx);
// /\w\d/g
```

* **Literal syntax** is written with forward slashes. It is quicker to write but can't be used to generate dynamic regular expressions. 

```js
const literal = /[^a-z]/g;
const wrong = /`${name}`/; // won't be dynamic
```

### Testing Patterns in JavaScript


* Every Regex object has a `.test()` method which takes in a string and returns `true` if there is a match between the Regex and the given string.

```js
const regex = /hi/;
const hasPattern = regex.test("I think Regex is the best!");
console.log(hasPattern); // true
```

### Finding Matches

* Every string has a `.match()` method which takes in a Regex object and returns an Array of matches between the string and the regular expression.
  * If the `g` flag is used, all results matching the complete regular expression will be returned, but capturing groups are not included.
  * If the `g` flag is not used, only the first complete match and its related capturing groups are returned.

```js
const str = 'this is my cat, "thing 1"';
const firstMatch = str.match(/hi/);
console.log(firstMatch);
// [
//   'hi',
//   index: 1,
//   input: 'this is my cat, "thing 1"',
//   groups: undefined
// ]

const allMatches = str.match(/hi/g);
console.log(allMatches);
// [ 'hi', 'hi' ]
```