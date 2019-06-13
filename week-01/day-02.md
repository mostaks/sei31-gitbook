---
description: JavaScript 101
---

# Day 02

What we covered today:

* Javascript - Introduction to Javascript
* Javascript - Data Types
* Javascript - Functions
* Javascript - Coding Conventions
* Javascript - Operators

### Slides <a id="slides"></a>

* ​[Variables and functions​](https://www.teaching-materials.org/javascript/slides/varsfunctions.html)
* ​[Javascript control flow and logical operators​ ](https://www.teaching-materials.org/javascript/slides/controlflow.html)

## Javascript - Introduction to Javascript <a id="javascript-introduction-to-javascript"></a>

> "Java is to Javascript, as ham is to hamster" - _Joel Turnbull_
>
> "There's a social contract that says: **we can't break the Web**." - _Joel Turnbull_

Javascript is the most popular programming language in the world by a long way. It works everywhere.

To be a programming language, a language needs to do the following things:

* Add 1 to a number;
* Check if a number is equal to zero;
* Branch - can alter flow of execution, or 'make decisions'.

**SOMETHING REALLY IMPORTANT**

> **“Programs must be written for people to read, and only incidentally for machines to execute.”**
>
> Harold Abelson, _Structure and Interpretation of Computer Programs_

There are two audiences that see any program that you will write. There is the computer, and there is the next human who sees the code. Prioritize the human! You need to make code readable and logical.

> "If given a program that works but is unreadable, and another that doesn't work but makes sense - I would take the one that doesn't work. I can still work with it and I understand it. It isn't a nightmare."
>
> _Joel Turnbull ...probably._

### _Javascript \(General\) - Recommended Readings_ <a id="javascript-general-recommended-readings"></a>

* ​[Eloquent Javascript](http://eloquentjavascript.net/)​
* ​[Speaking Javascript](http://speakingjs.com/)​
* ​[Javascript: The Good Parts](http://bdcampbell.net/javascript/book/javascript_the_good_parts.pdf)​
* ​[W3: A Short History of Javascript](https://www.w3.org/community/webed/wiki/A_Short_History_of_JavaScript)​

### History <a id="history"></a>

* 1995 - At Netscape, Brendan Eich created "JavaScript".
* 1996 - Microsoft releases "JScript", a port for IE3.
* 1997 - JavaScript was standardized in the "ECMAScript" spec.
* 2005 - "AJAX" was coined and the web 2.0 age begins.
* 2006 - jQuery 1.0 was released.
* 2010 - Node.JS was released.
* 2015 - ECMAScript 6 was released.

## Javascript <a id="javascript"></a>

## Declaring variables <a id="declaring-variables"></a>

Pre-ES6 \(ECMA2015\), variables were declared using `var`. However, ES6 introduced two new ways of declaring variables: `const` and `let`, which forces us to think about the variable before we declare it and makes debugging easier.

`const` allows you to declare a variable with a value which cannot be changed. It is scoped at the block level.

`let` allows you to do the same as `const` without the single assignment constraint.

In this course, we recommend you use `const` at first instance, and if you need to change the value of a variable, to instead declare your variable with `let`.

## Data Types <a id="data-types"></a>

### Primitive Value Types <a id="primitive-value-types"></a>

**Strings** - An immutable string of characters denoted by \(back ticks\), " " \(double quotation marks\) or ' ' \(single quotation marks\),

```javascript
const greeting = "Hello Kitty";
const restaurant = `McDonalds`;
​
​greeting;
// => "Hello Kitty"
```

**Numbers** - Whole numbers \(6, -102\) or floating point numbers \(5.8727\)

```javascript
const myAge = 35;
const roughPi = 3.14;​
​
myAge;
// => 35
```

**Boolean** - Represents the logical values true or false.

```javascript
const catsAreBest = true;
const dogsRule = false;​
​
catsAreBest;
// => true
dogsRule;
// => false
```

**undefined** - Represents a value that has not been defined.

```javascript
const myBiceps;​
​
myBiceps;
// => undefined
```

**null** - Represents something that is explicitly undefined.

```javascript
const goodPickupLines = null;​
​
goodPickUpLines;
// => null
```

### Variable Names <a id="variable-names"></a>

* Must begin with letters, $ or \_
* Can only contain letters, numbers, $ and \_
* Are case sensitive
* Do not use [reserved words](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords):
  * NOTE: A variable name can _include_ a reserved word but cannot be comprised solely of a reserved word - using the reserved word 'for' as an example, `var form` is okay; `var for` is NOT okay\).
* Choose clarity and meaning
* Prefer _camelCase_ for multipleWordsInVariableNames \(instead of _snake\_case_\). _dash-case_ cannot be used since variable names cannot include the '-' character
* Pick a naming convention and stick with it

```javascript
// All Good!
let numPeople, $mainHeader, _num, _Num;​
​
// Nope.
let 2coolForSchool, soHappy!;
```

Everything in Javascript returns the result of an expression.

```javascript
4 + 2;
// => 6​
​
8;
// => 8​
​
console.log("Hello World.")
// Passes a string into the console.log function. Then returns the result of this 
//expression into the console.
```

Variables store the result of expressions as well.

```javascript
let courseName = "S" + "E" + "I";
courseName;
// => "SEI"
​
let testMultiplication = 5 * 7;
testMultiplication;
// => 35​
​
let x = 2 + 2;
x;
// => 4
​
let y = x * 3;
y;
// => 12
​
​let name = "Claire";
let greeting = "Hello " + name;
let title = "Baroness";
let greeting = greeting + ', ' + title;
greeting;
// => "Hello Claire, Baroness"​
​
let name = "Pamela";
let greeting = "Hello " + name;
greeting;
// => "Hello Pamela"
```

Javascript is a _Loosely Typed_ language.

What this means is that we do not explicitly state what type of value will be stored by a variable - Javascript will figure out the type of value to be stored by looking at the _type of the result of the expression_. If that sounds confusing, remember:

* `5` is an expression that evaluates to the **number** 5;
* `"5"` is an expression that evaluates to the **string** "5";
* `5 + 5` is an expression that evaluates to the **number** 10;
* `5 + "55"` is an expression that evaluates to the **string** "555"

The result of the expression `10` is the number 5, so `let y = 5` will give y the type of `number`.

The result of the expression `5 + "cats"` is "5cats", so var `let z = 5 + "cats"` will give z the type of `string`.

A variable can be of only one type at one time, but the type of a particular variable can be reassigned.

```javascript
// When the variable is declared, it is assigned the type of 'number', but is 
//subsequently changed to 'string'
let y = 2
console.log(typeof(y))
// number
y = 2 + ' cats';
console.log(typeof y);
// string
```

For a much more complex overview of the differences between loosely typed and strongly typed languages, [see here.](http://en.wikipedia.org/wiki/Strong_and_weak_typing)​

**String Interpolation**

String interpolation is available as part of ES6, denoted by backticks. To do this, we use `${}` to insert variable names into the string. This saves us from adding discrete strings together with `+`

Backticks also allows us to have multi-line strings \(not available when using quotation marks\), which helps with the readability of your code.

```javascript
// Simple string substitution
var name = "Michelle"; // or `Michelle`
console.log(`Hello, ${name}!`);​
// => "Hello, Michelle!"​
​
// Multiple line Strings​
const firstName = 'Jane';
console.log(`Hello ${firstName}!
  How are you
  today?`);​
​
// Output:
// Hello Jane!
// How are you
// today?
```

**Let's take a moment...**

You know how they say you need to write 10 000 words before you write a novel? All of the stuff you are learning right now is a part of that 10 000 words... if learning to code were writing a novel...

### _Javascript Variables - Exercises_ <a id="javascript-variables-exercises"></a>

* ​[Variables: Exercises​](https://gist.github.com/wofockham/901da450091e0e08af12)

### Comments <a id="comments"></a>

Comments are human-readable lines of text that the computer will ignore. Comments are _incredibly_ useful for leaving notes to yourself \(and other developers\) in your code - these comments might explain the purpose of a section of code or how a particularly complex function works, note any bugs in the code, or leave 'TODO' notes for yourself.

In Atom, `<CMD> + /` will toggle the comments. Highlight a block of text and try it out.

```javascript
// This is a single line comment in JS​
​
/*    
  This is a block level comment    
  i.e. multiline
*/

```

## Javascript - Functions <a id="javascript-functions"></a>

Functions are way to make a collection of statements re-usable. This is the most powerful thing in Javascript.

**Declaring functions:** You need to declare functions before you can call them:

```javascript
// For future reference, this is a 'function declaration' (more on this below)
function sayMyName () {    
  console.log( "Hello Jane" );
}​
​
// But since functions are also data types, they can be stored in a variable!
// This is my favourite way of declaring functions - generally stick to this.
// For future reference, this is known as a 'function expression' (more on this below)
const sayMyName = function () {    
  console.log( "Hello Jane" );
}
```

**Calling functions:** Once we've declared a function, we can call it using the function name followed by parentheses:

```javascript
sayMyName();
// => "Hello Jane"
```

If you want to see the difference between function declarations and function expressions, see [this blog post](https://javascriptweblog.wordpress.com/2010/07/06/function-declarations-vs-function-expressions/) and [this StackOverflow post](http://stackoverflow.com/questions/1013385/what-is-the-difference-between-a-function-expression-vs-declaration-in-javascrip) .

### Parameters and Arguments <a id="parameters-and-arguments"></a>

The terms 'parameter' and 'argument' are often used interchangeably - the context usually makes the meaning clear, and only a real jerk would pull you up on it - but basically:

* Parameters are used to define a function;
* Arguments are used to invoke a function.

Functions in Javascript can accept as many named parameters \(or arguments\) as we want. We can then access and use those arguments from within the function. This is crazily powerful.

```javascript
const sayMyName = function ( firstName, lastName ) {    
  console.log( "Hello, " + firstName + " " + lastName + "!");
}​
​
sayMyName( "Jane", "Birkin" );
// => "Hello Jane Birkin!"​
​
// We don't have to pass in plain data types, we can also pass in variables as arguments
const s = "Serge";
const g = "Gainsbourg";​
​
sayMyName( s, g );
// => "Hello Serge Gainsbourg!"
```

### Return values <a id="return-values"></a>

Functions wont automatically return the evaluation of the code within itself. You will need to add a return statement at the end of the function.

```javascript
const addNumbers = function ( num1, num2 ) {  
  result = num1 + num2;  
  return result;
}
const sum = addNumbers(5, 2);
​
​sum;
// => 7
```

### Scope <a id="scope"></a>

Javascript Variables have "function scope". They are visible only within the function in which they were declared.

In the example below, the variable `localResult` has "local scope" \(ie, it can only be used/accessed within the `addNumbers` function\):

```javascript
var addNumbers = function ( num1, num2 ) {  
  var localResult = num1 + num2;  
  console.log( "The local result is: " + localResult );
}
​
​addNumbers( 5, 7 );
// => "The local result is 12"   
// Great. This worked, but...
​
​console.log( localResult );
// => undefined
// This returned 'undefined' because localResult is defined inside the addNumbers() 
//function - we can't see/use localResult "outside" of the addNumbers function.
```

But what if we wanted to access a variable that is assigned within a function from _outside_ that function? We can declare the variable with "global" scope \(outside the function\), then assign a value to that variable within the scope of the function.

In the example below, the variable `globalResult` below has "global scope":

```javascript
var globalResult;
var addNumbers = function ( num1, num2 ) {  
  globalResult = num1 + num2;  
  console.log( "The global result is: " + globalResult );
}
​
​addNumbers( 5, 7 );
// => "The global result is 12"
​
console.log( globalResult );
// => 12
// Because this wasn't defined in the function, it is available outside the function.
```

If we assign a value to a variable inside a function without declaring the variable with `var`, the variable - which would ordinarily have local/function scope \(ie, only be accessible within that function\) - will become a global variable. This can cause a lot of problems - if you want a variable to be global, explicitly declare it globally.

**Quick recap: three things to remember about functions:**

* You can pass things into functions as parameters/arguments
* You can return things from a function \(to allow for chaining or usage in expressions\)
* Variables declared in a function \(or passed in as parameters to a function\) have local scope \(or function scope\) - i.e. they are only accessible within that function. Variable scope is something important to get your head around.

### _Javascript Variable Scope - Recommended Readings_ <a id="javascript-variable-scope-recommended-readings"></a>

For a more in-depth dive into Javascript variable scope, see:

* ​[Speaking Javascript \(O'Reilly\) - Ch 1: Basic Javascript](http://speakingjs.com/es5/ch01.html#basic_var_scope_and_closures) - this is very good.
* ​[Blog Post: Everything You Wanted To Know About Javascript](http://toddmotto.com/everything-you-wanted-to-know-about-javascript-scope/)​
* ​[Sitepoint - Demystifying Javascript Variable Scope and Hoisting](http://www.sitepoint.com/demystifying-javascript-variable-scope-hoisting/?utm_source=SitePoint&utm_medium=email&utm_campaign=Versioning)​

### _Javascript Functions - Exercises_ <a id="javascript-functions-exercises"></a>

Now that we know a bit more about functions, have a crack at the following exercises:

* ​[Functions: Exercises​](https://gist.github.com/wofockham/0cba52fac6cff19a3010)

## Javascript - Coding Conventions <a id="javascript-coding-conventions"></a>

Javascript is pretty flexible. We can use whatever terrible formatting we like and a computer will \(generally\) be able to understand it. But for maximum readability, it's important to follow conventions. There a whole bunch of these, and there are disagreements about some, but some generally accepted conventions are:

* Use newlines between statements;
* Use indentation to show blocks;
* Use camelCase for variable names \(as discussed above\).

```javascript
// BAD
function addNumbers(num1,num2) {return num1 + num2;}​
​
function addNumbers(num1, num2) {
return num1 + num2;
}​
​
// GOOD
const addNumbers = function(num1, num2) {  
  return num1 + num2;
}
```

### _Javascript Coding Conventions - Recommended Readings_ <a id="javascript-coding-conventions-recommended-readings"></a>

For information relating to javascript style, see:

* ​[Idiomatic JS](https://github.com/rwaldron/idiomatic.js) - the best style guide;
* ​[Javascript Style Guides and Beautifiers](http://addyosmani.com/blog/javascript-style-guides-and-beautifiers/)​
* ​[AirBnB Style Guide](https://github.com/airbnb/javascript?utm_source=javascriptweekly&utm_medium=email) - this is also quite good.

## Javascript - Operators <a id="javascript-operators"></a>

### Comparison Operators <a id="comparison-operators"></a>

Use these operators to compare two values for equality, inequality or difference.

| Operator | Meaning | True Statements |
| :--- | :--- | :--- |
| == | Equality | 28 == '28', 28 == 28 |
| === | Strict Equality | 28 === 28 |
| != | Inequality | 28 != 27 |
| !== | Strict Inequality | 28 !== 23 |
| &gt; | Greater than | 28 &gt; 23 |
| &gt;= | Greater than or Equal to | 28 &gt;= 28 |
| &lt; | Less than | 24 &lt; 28 |
| &lt;= | Less than or Equal to | 24 &lt;= 24 |

We can use all of these in any statement or evaluation. Comparison operators are very useful in **if statement** conditionals, etc.

We would generally want to use the **strict** equality/inequality operators - they compare both type \(i.e. strings, numbers etc.\) and content, not just content. For example:

**Strict Equality and Inequality**

```javascript
5 === "5"
// => false​
​
5 !== "5"
// => true
```

**Equality and Inequality**

```javascript
5 == "5"
// => true
​
​5 != "5"
// => true
```

### Logical Operators <a id="logical-operators"></a>

These are typically used in conjunction with the comparison operators and are commonly used to group multiple conditions, or for special types of variable declarations.

| Operator | Meaning | True Expressions |
| :--- | :--- | :--- |
| && | AND | 4 &gt; 0 && 4 &lt; 10 |
| \|\| | OR | 4 &gt; 0 \|\| 4 &lt; 3 |
| ! | NOT | !\(4 &lt; 0\) |

In Javascript, any/all logical and comparison operators can be used in any control flow statement.

**NOTE:** Empty strings \(`""`\), `0`, `undefined`, `NaN` and `null` are all considered false in Javascript!

If you want to find out everything there is to know about Javascript Equality, see [this .](http://dorey.github.io/JavaScript-Equality-Table/)​

## Truthy vs Falsey <a id="truthy-vs-falsey"></a>

If you don't use a comparison or logical operator, JS tries to figure out if the value is "truth-y".

```javascript
// all numbers are truthy (including negative numbers) except 0
if(3){
  console.log("It is truthy.")
}
// => "It is truthy."
​
if(-3){
  console.log("It is truthy.")
}
// => "It is truthy."
​
if(0){
  console.log("It is truthy.")
}
// => no output
​
//all strings are true, except empty string
if("false"){
  console.log("It is truthy.")
}
// => "It is truthy."
​
if(""){
  console.log("It is truthy.")
}
// => no output
​
//Value false, undefined, null are falsey
if(false){
  console.log("It is truthy.")
}
// => no output
​
if(undefined){
  console.log("It is truthy.")
}
// => no output
​
if(null){
  console.log("It is truthy.")
}
// => no output
```

## Homework <a id="homework"></a>

* [​Calculator and Strings​](https://gist.github.com/wofockham/8f953ac7f33125898071)
* Bonus:
  * Jump on [codewars](https://www.codewars.com/)

​

