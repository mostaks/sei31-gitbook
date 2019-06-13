# Day 04

What we covered today:

* ​Warmup and solution - Serge - JS​​
* JavaScript - Objects

### Slides <a id="slides"></a>

* [​Javascript Objects](https://www.teaching-materials.org/javascript/slides/objects)​ 

### Classwork <a id="classwork"></a>

* ​Objects Exercises

### Warmup! 🧙🏻‍♂️

* [Serge Says](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week01/day04_sergeSays)

## JavaScript - Collections - Objects <a id="javascript-collections-objects"></a>

In JavaScript, an object is a standalone entity - filled with properties and types \(or keys and values\). It is very similar in structure to a dictionary.

So, most javascript objects will have keys and values attached to them - this could be considered as a variable that is attached to the object \(also allows us to iterate through them\).

They are sometimes called "associative arrays". Remember that they are not stored in any particular order \(they can change order whenever\).

### How to create an object <a id="how-to-create-an-object"></a>

```javascript
// With object literal
const newObject = {};
​
​// Using Object
const newObject = new Object();
```

### How to add properties to an object <a id="how-to-add-properties-to-an-object"></a>

```javascript
// Remember to seperate by commas!
const newObject = {  
  objectKey: "Object Value",  
  anotherObjectKey: "Another Object Value",  
  objectFunction: function () {​ 
     
  }
};​
​
const newObject = {};
newObject.objectKey = "Object Value";
newObject.objectFunction();
newObject["anotherObjectKey"] = "Another Object Value";​
​
// Can also use Constructors and Factories - will see them in Week 1 Day 5 notes.
```

### How to access properties of an object <a id="how-to-access-properties-of-an-object"></a>

There are two ways to access the properties of an object:

* **Dot notation** - syntax: `object.propertyName`
* **Square bracket notation** - `object["propertyName"]`

Remember: like all JS variables - both the object name and property names are case sensitive.

```javascript
const favouriteCar = {  
  manufacturer: "Jaguar",  
  year: 1963,  
  model: "E-Type"
}​
​
// Dot notation access to object properties
favouriteCar.year​
​
// Square bracket notation access to object properties
favouriteCar["year"]
​
//however, if we create a new variable 
const key = "year"
key 
//=> "year"
//if we run the code via dot notation
favouriteCar.key //=> undefined
// via breacket(array-style) notation
favouriteCar[key] //=> 1963
​
//the things inside [] get evaluated
```

* ​[Objects: Exercises​](https://gist.github.com/wofockham/3ed2b7662e0e067d4e2a)

### How to iterate through an object <a id="how-to-iterate-through-an-object"></a>

```javascript
Object.keys(newObject); // Returns an array of all the keys in the specified object.
Object.getOwnPropertyNames(newObject); // So does this
​
​const obj = {  
  a: 1,  
  b: 2,  
  c: 3
};​
​
//we use bracket notation to get value
for (let key in obj) {  
  console.log( `key: ${key}, value: ${obj[key]}`);
}
// key: a, value: 1
// key: b, value: 2
// key: c, value: 3
​
//however, for dot notation
for (let key in obj) {  
  console.log( `key: ${key}, value: ${obj.key}`);
}
// key: a, value: undefined
// key: b, value: undefined
// key: c, value: undefined
​
//if using dot notatin, it will literally looking for the key called "key".
```

### Deleting properties of an object <a id="deleting-properties-of-an-object"></a>

```javascript
const favouriteCar = {  
  manufacturer: "Jaguar",  
  year: 1963,  
  model: "E-Type"
}​
​
delete favouriteCar.year;
favouriteCar
//=>  { manufacturer: "Jaguar", model: "E-Type"}​    
```

### Comparing objects <a id="comparing-objects"></a>

In JavaScript objects are a reference type. Two distinct objects are never equal, even if they have the same properties. Only comparing the same object reference with itself yields true.

```javascript
// Two variables, two distict objects with the same properties
const fruit = { name: "apple" };
const fruitbear = { name: "apple" };
​
​fruit == fruitbear;
// => false
fruit === fruitbear;
// => false​
​
// Two variables, a single object
const fruit = { name: "apple" };
const fruitbear = fruit;  // Assigns fruit object reference to fruitbear
​
​// Here fruit and fruitbear are pointing to same object
fruit == fruitbear;
// => true
fruit === fruitbear;
// => true
```

There's no simple way to compare objects using 'vanilla' JavaScript, but there are some JavaScript libraries that make object comparison easier - UnderscoreJS, for example, has an method - "\_.isEqual", which tests equality based on object properties. There are lots of alternatives - for example, check out [this](http://stackoverflow.com/questions/1068834/object-comparison-in-javascript) Stack Overflow thread - but I would stick to Underscore's method. [Underscore](http://underscorejs.org/) is a great JavaScript library which we'll be talking about it in great detail later on.

## Homework <a id="homework"></a>

* ​[​Javascript Bank​](https://gist.github.com/wofockham/dacf2da17c743afb2b17)​
* Bonus - Read:
  * ​[MDN - Arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) - MDN is an amazing resource for JavaScript, HTML and CSS.
  * ​[JavaScript Tutorials - Arrays](http://javascript.info/tutorial/array)​
  * ​[Speaking JavaScript - Arrays](http://speakingjs.com/es5/ch01.html#basic_arrays)​

\_\_

