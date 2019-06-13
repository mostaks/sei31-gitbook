---
description: It is this this that is the this this is
---

# Day 05

What we covered today:

* Javascript - Review
* Javascript - Scope
* JavaScript - Advanced Objects
  * Methods
  * Factories and Constructors
* JavaScript - Advanced Functions
  * The `this` Keyword
  * The `arguments` Object

### Warmup

* [Leap Year](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week01/day05_leapYear)

### Slides

* [Review](https://www.teaching-materials.org/_deprecated/jsreview/#/)

### Classwork

* [Scope](https://github.com/wofockham/sei-31/tree/master/01-javascript/scope)
* [Factories](https://github.com/wofockham/sei-31/tree/master/01-javascript/factories)

## Javascript - Question Time: How to make console.log\(\) output colourful and stylish?

You can simply use CSS properties and modify accordingly.

To add styles to logs, the method expects `%c` within the first argument of `console.log()` and it picks up the very next argument as CSS style for the `%c` pattern argument text. 

```javascript
console.log("%cThis is a %cConsole.log", "background:black ; color: white", "color: red; font-size:25px");

//If you want to add multiple style, then simply add multiple %c text and add style arguments respectively.

console.log("%c first one %c second one", "background:black ; color: white", "background:blue; color: yellow");

//Please note, there are two %c pattern text and hence two CSS style arguments. And the CSS arguments are picked up respectively from left to right.
```

### _Further Reading_

* [Style & Colour in browser's console.log output](http://voidcanvas.com/make-console-log-output-colorful-and-stylish-in-browser-node/)
* [How to get the most out of the JavaScript console](https://medium.freecodecamp.org/how-to-get-the-most-out-of-the-javascript-console-b57ca9db3e6d)

## JavaScript - Advanced Objects <a id="javascript-advanced-objects"></a>

### Methods <a id="methods"></a>

In JavaScript, functions are first-class. This means that the language supports:

* Constructing new functions during the execution of a program;
* Passing functions into other functions as arguments, and \(relevantly\);
* **Storing functions in data structures**;

Since we can store functions in data structures, we can store a function as the property of an object - a function stored as the property of an object is called a 'method'.

We've already seen a bunch of methods - for example, when we call `.toString()` on a number, we are actually calling the `.toString()` method of the global Number object \(which is a property of that object\).

### Factories and Constructors <a id="factories-and-constructors"></a>

First off, both **constructors** and **factories** are "blue prints". They bootstrap development. Often they are more hassle than they are worth though, so be wary. Think about whether all the code necessary to get a factory or constructor efficiently is actually worth it. Object literals can get you through 95% of the time.

### Factories <a id="factories"></a>

We can use 'factory' functions to create objects.

```javascript
const createCat = function(name, age, breed, food) {​  
​
  // Our factory can include a default value for the objects it produces  
  if (food === undefined) {    
    food = "Bacon";  
  }  
  return {    
    name: name,    
    age: age,    
    breed: breed,    
    // Building on our knowledge of functions in objects (ie, methods), 
    //lets add a few methods to objects produced by this factory    
    meow: function() {      
      console.log("Meooooow");    
    },    
    eats: function(food) {      
      console.log(`Lewis eats ${food}`);    
    }  
  };
};​
​
const lewis = createCat("Lewis", 1, "Maine Coon", "Strawberries");​
​
const cats = [  
  createCat("Audrey", 1, "Domestic Shorthair", "Tuna"),  
  createCat("Lewis", 1, "Maine Coon", "Strawberries"),  
  createCat("Cooper", 1, "Domestic Shorthair")
]
```

### Constructors <a id="constructors"></a>

Constructors are essentially factories for objects that are invoked via the `new` operator. When a constructor is invoked using the `new` operator, a constructor will:

* Create a new object
* Set `this` within the function to that object
* Return the object.

Normally, JS objects are only maps from strings to values, but JS also supports prototypal inheritance - something that is truly object-oriented. They are quite similar to classes in other languages.

#### _What does it entail?_ <a id="what-does-it-entail"></a>

The initial set up - this is where you set up the instance data...

```javascript
const Point = function ( x, y ) {    
  this.x = x;    
  this.y = y;
};
```

The application of methods or functions... These will apply to any instance.

```javascript
Point.prototype.dist = function () {    
  return Math.sqrt( this.x * this.x + this.y * this.y );
};
```

The invocation of a new instance.

```javascript
let p = new Point( 3, 4 );
console.log( `Point X: ${p.x}` );
```

We can also check to see if an object is an instance of a constructor:

```javascript
typeof p
// Returns "object"
​
​p instanceof Point
// true
```

### _JavaScript - Advanced Objects - Factories and Constructors - Further Reading_ <a id="javascript-advanced-objects-factories-and-constructors-further-reading"></a>

* ​[Phrogz - OOP in JS - Inheritence](http://phrogz.net/JS/classes/OOPinJS2.html)​

## Javascript - Advanced Functions <a id="javascript-advanced-functions"></a>

### The `this` keyword \(AKA self\) <a id="the-this-keyword-aka-self"></a>

The `this` keyword is one of the most powerful things in JavaScript, but also one of the hardest to understand. It gets insanely complicated, and there are a lot of exceptions to the simplistic generalizations below.

In JavaScript, `this` will always refer to the owner of the function we are executing. If the function is not within an object, or another function, `this` will refer to the global object \(or window\). Window is an object that exists in every browser, applying keys and values to this object will make them globally accessible. Don't do it regularly though.

```javascript
// GLOBAL THIS //
const doSomething = function () {    
  console.log( this );    
  // Will log the window object
}​
​
// OBJECT THIS //
const objectFunction = {    
  testThis: function () {        
    console.log( this );        
        // Would log the objectFunction object    
    }
}
objectFunction.testThis();​
​
// EVENT THIS //
const button = document.getElementById("myButton");
// This is a basic click handler - you aren't expected to understand this yet!
button.addEventListener( "click", function() {    
  console.log( this );    
  // Will log the HTML element that this event ran on (button with id myButton)
});

```

### A simplistic generalization of `this` <a id="a-simplistic-generalization-of-this"></a>

* In a simple function \(one that isn't in another function or object\) - `this` stays as the default - window.
* In a function that is within an object, `this` is defined as the object - it's immediate parent.
* In an event handler \(a function that is called based on browser interaction\), `this` is defined as the element that was interacted with.

The `this` keyword is really useful when we have a function that accepts an object as an argument - we can use `this` to access properties of the object.

### Global / default binding <a id="global-default-binding"></a>

```javascript
sayHello();
```

### Implicit binding <a id="implicit-binding"></a>

When we call a function through an object, `this` becomes the object.

```javascript
dog.sayHello();
```

### Explicit binding <a id="explicit-binding"></a>

```javascript
const sayHello = function() {    
  console.log("Hello, " + this.name);
}​
​
const person = {    
  name: "Lewis"
}​
​
// Using the .call method
sayHello.call(person);
// => "Hello, Lewis"
​
​// Using the .apply method
sayHello.apply(person);
const hi = sayHello.bind(person);
// This creates a new function where the keyword "this" will always represent the 
//person object passed into the .bind method.
hi();
// => "Hello, Lewis"
// Creates a new function where the keyword 'this' will always represent 'person', 
//and can be called using hi();
```

### `new` binding <a id="new-binding"></a>

```javascript
const Person = function(name) {    
  this.name = name;    
  this.sayHello = function() {        
    console.log("Hello, " + this.name);    
  }
}​
​
const lewis = new Person("Lewis");​
​
lewis.name
// => "Lewis"
lewis.sayHello();
// => "Hello, Lewis"
```

### _JavaScript -_ `this` _- Recommended Reading_ <a id="javascript-this-recommended-reading"></a>

* ​[REPL - Lizzie the Cat](https://repl.it/lwW/1)​
* ​[Kyle Simpson - You Don't Know JS - `this` and Object Prototypes](https://github.com/getify/You-Dont-Know-JS/tree/master/this%20%26%20object%20prototypes)​
* ​[Todd Motto - Understanding the 'This' Keyword in JavaScript](http://toddmotto.com/understanding-the-this-keyword-in-javascript/)​
* ​[MDN - This](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)​
* ​[JavaScript is Sexy - Understand JavaScript's 'This' With Clarity](http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/) ​
* ​[Quirks Mode - This](http://www.quirksmode.org/js/this.html)​

### JavaScript - The `arguments` object <a id="javascript-the-arguments-object"></a>

The `arguments` object is an array-like object that can be used to access the arguments passed into a function, regardless of whether they match named parameters.

#### _Using 'arguments' to create default arguments_ <a id="using-arguments-to-create-default-arguments"></a>

If we call a function without passing in arguments to match the number of named parameters, we run into `undefined` problems.

```javascript
const nameImprover = function (name, adj) {    
  return `Col ${name} Mc${adj} pants`;
};​
​
nameImprover("Badger");
// => "Col Badger Mcundefined pants"
```

We can check whether any of the arguments are undefined, and set a default value if so:

```javascript
const nameImprover = function (name, adj) {  
  if (adj === undefined) {    
    adj = "Fancy"  
  }    
  return 'Col ' + name + ' Mc' + adj + ' pants';
};​
​
nameImprover("Badger");
// => "Col Badger McFancy pants"
```

#### _Using_ `arguments` _to create variadic functions_ <a id="using-arguments-to-create-variadic-functions"></a>

A **variadic function** is a function where the total number of parameters is unknown when the function is declared, and can vary when the function is called. We can use the arguments object to create variadic functions in JavaScript.

```javascript
//Create a function that will return the sum of any and all numbers passed in as 
//arguments
const addNumbers() {  
  let sum = 0;  
  for (let i = 0; i < arguments.length; i++) {    
    sum += arguments[i];  
  }  
  return sum;
}​
​
addnumbers(3,5);
// => 8​
​
addNumbers(3,5,7,9);
// => 24
```

## Homework

* [MTA​​](https://gist.github.com/wofockham/8ac3c1d747f345d89d3d)
* ​[JavaScript Readings](https://gist.github.com/wofockham/8a702a9bf0a1456df7d4)​



