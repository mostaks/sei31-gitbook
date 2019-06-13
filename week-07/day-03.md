---
description: _ and CORS
---

# Day 03

What we covered today:

* More Flickr Search
* [Underscore.js](https://github.com/wofockham/sei-31/tree/master/11-underscore/underscore-intro)
* AJAX API

Warmup

* [Atbash Cipher](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week07/day03_atbash_cipher)

## Flickr API w/ AJAX <a id="flickr-api-w-ajax"></a>

Set up your environment and create all required files.

```bash
mkdir Flickr
cd Flickr
touch index.html
mkdir js
touch js/jquery.js # remember that you will actually have to curl the jquery file with it's contents or create a script tag in your index.html linking the CDN
touch js/underscore.js
touch js/search.js
mkdir css
touch css/style.css
```

Set up form in index.html

* Use Emmet to autofill your page with `! + tab`

Add a script/link tag to the head to link both the `search.js` and `style.css`

```markup
  <script src="js/main.js" charset="utf-8"></script>
```

```markup
  <link rel="stylesheet" type="text/css" href="style.css">
```

```markup
<div class="container">
    <form>
        <input type="search" id="query">
        <button>Search</button>
    </form>
    <div id="images"></div>
</div>
```

Add a document ready function with listener on form waiting for a submit in search.js file

```javascript
$(document).ready(function(){
    $("search").on("submit", function(e) {
        e.preventDefault(); //Stay on this page, we'll do the AJAX request ourselves
        console.log("form submitted");
    };
};
```

Target the value of the input and assign it to a variable called query

```javascript
$(document).ready(function(){
    $('search').on('submit', function(e) {
        e.preventDefault(); //Stay on this page, we'll do the AJAX request ourselves
​
        const query = $('#query').val();
​
        console.log(query);
        searchFlickr( query );
    };
};
```

Create another function outside of `document ready` function that will send a request to the Flickr API

```javascript
const searchFlickr = funciton( term ) {
    console.log( `Search Flickr for: ${ term }` );
};
```

Fetch images from Flickr using AJAX with the below arguments

```javascript
const flickrURL = 'https://api.flickr.com/services/rest/?jsoncallback=?';
​
$.getJSON(flickrURL, {
    method: 'flickr.photos.search',
    api_key: 'YOUR_API_KEY_HERE',
    text: term,
    format: 'json'
}).done(function(results){
    console.log(results);
});
```

Create another function that generate the URL

```javascript
const generateURL = function(photo){
​
    return [
​
        'http://farm',
        photo.farm,
        '.static.flickr.com/',
        photo.server,        
        '/',
        photo.id,
        '_',
        photo.secret,
        '_q.jpg' //Change 'q' for different sizes
    ].join('');
​
};

```

Change the end of the AJAX request to include a `.done` argument and call the `showImages` function

```javascript
$.getJSON(flickrURL, {
    method: 'flickr.photos.search',
    api_key: 'YOUR_API_KEY_HERE',
    text: term,
    format: 'json'
}).done(showImages);
```

Define the `showImages` function between the searchFlickr and generateURL functions

```javascript
const showImages = function (results){
​
    _(results.photos.photo).each(function (photoInfo) {
        const photoURL = generateURL(photoInfo);
        console.log( photoURL );
    });
​
};
```

Create a new image and set the image `src` to be the `photoURL`

```javascript
_(results.photos.photo).each(function (photoInfo) {
    const photoURL = generateURL(photoInfo);
​
    const $img = $('</img>', { src: photoURL });
​
});
```

Append the new image to the page

```javascript
_(results.photos.photo).each(function (photoInfo) {
    const photoURL = generateURL(photoInfo);
    const $img = $('</img>', { src: photoURL });
​
    $img.appendTo('#image'); // another option would be  $('#images').append($img);
});
```



## JavaScript - Underscore <a id="javascript-underscore"></a>

### What is Underscore? <a id="what-is-underscore"></a>

It's a programmer's tool-belt developed by a guy called [Jeremy Ashkenas](https://twitter.com/jashkenas) \(who also made Backbone and CoffeeScript\). It provides a whole range of useful utility functions that you will end up using thousands and thousands of times.

The library is _tiny_, with the minified production code coming in at just 5.7kb, which is pretty cool for a library that is so handy.

You'll notice that a lot of Underscore methods match Ruby methods you're already familiar with. If there's a Ruby method you're really fond of, there's a reasonable chance that Underscore has a corresponding method \(helpfully, they might even share the same name \(though, of course, our snake\_cased method names are replaced with camelCased ones\)\).

### Underscore functions <a id="underscore-functions"></a>

Underscore's functions are broken down into six categories:

* ​[Collections](http://underscorejs.org/#collections)​
* ​[Arrays](http://underscorejs.org/#arrays)​
* ​[Objects](http://underscorejs.org/#objects)​
* ​[Utilities](http://underscorejs.org/#utility)​
* ​[Functions](http://underscorejs.org/#functions)​
* ​[Chaining](http://underscorejs.org/#chaining)​

### Where can I get it? <a id="where-can-i-get-it"></a>

* ​[Underscorejs.org](http://underscorejs.org/) - from the source
* ​[cdnjs](https://cdnjs.com/libraries/underscore.js) - using a CDN
* ​[underscore-rails](https://github.com/rweng/underscore-rails) - in a Ruby gem for Rails \(see below\)

### Using Underscore with Rails <a id="using-underscore-with-rails"></a>

* Add this to your Gemfile - `gem 'underscore-rails'`
* Run `bundle`
* Add this line before `require tree` - `//= require underscore`

### Annotated examples <a id="annotated-examples"></a>

The annotated examples below are useful to:

* Read an explanation of the most common Underscore method
* See a working example of the method
* Try it out yourself

### Common methods - arrays and objects <a id="common-methods-arrays-and-objects"></a>

#### \__.each_ <a id="_-each"></a>

* Iterates through each thing in the passed in collection
* **\_.each\( collection, iteratee\_function, \[context\] \);**
* The iteratee function receives an element, an index, and en entire collection as parameters
* \[context\] is an optional parameter that you probably won't use much
* \_.each can be used on both arrays and objects, but always returns an array.

```javascript
// "EACH WITH ARRAY"
const arr = [ 1, 2, 3 ];
​
_.each( arr, function ( element, index, entire_array ) {
  console.log( "Element: ", element );
  console.log( "Index: ", index );
  console.log( "Entire Array: ", entire_array );
});
​
// "EACH WITH OBJECT"
const obj = {
  four: 4,
  five: 5,
  nine: 9
};
​
_.each( obj, function ( value, key, entire_object ) {
  console.log( "Value: ", value );
  console.log( "Key: ", key );
  console.log( "Entire Object: ", entire_object );
});
​
​
const bros = "Groucho Chico Harpo".split(' ');
​
_(bros).forEach(function (b){
  console.log( b.toUpperCase());
});
​
// Iterating over Objects
const groucho = {
  name: 'Groucho',
  instrument: 'guitar',
  vice: 'cigars'
};
​
_(groucho).forEach(function ( value, key ){ // remember that the value comes first
  console.log(`${ key } is ${ value }`);
});
```

#### \__.map_ <a id="_-map"></a>

* Iterates through each thing in the passed in collection and passes back an altered collection.
* **\_.map\( collection, iteratee\_function \);**
* The iteratee function receives an element, an index, and en entire collection as parameters

```javascript
// "MAP WITH ARRAY"
const arr = [ 1, 2, 3 ];
​
const arrByFive = _.map( arr, function ( element, index, entire_array ) {
  return element * 5;
});
console.log( "Original Array: ", arr );
console.log( "Array By Five: ", arrByFive );
​
// "MAP WITH OBJECT"
const obj = {
  one: 1,
  two: 2,
  three: 3
};
​
const objByFive = _.map( obj, function ( element, index, entire_array ) {
  return element * 5;
});
console.log( "Original Object: ", arr );
console.log( "Object By Five (turned into an array by map): ", objByFive );
```

#### \__.reduce_ <a id="_-reduce"></a>

* Iterates through each thing in the passed in collection and returns a single sum
* **\_.reduce\( collection, iteratee\_function, starting\_value \)**;
* The iteratee function receives a sum, a current value, and en entire collection as parameters

```javascript
// "REDUCE WITH ARRAY"
const arr = [ 1, 2, 3 ];
​
const reducedArr = _.reduce( arr, function ( sum, value, index, list ) {
  return sum, value;
}, 0 );
console.log( "Original Array: ", arr );
console.log( "Reduced Array (to a sum): ", reducedArr );
​
// "REDUCE WITH OBJECT"
const obj = {
  one: 1,
  two: 2,
  three: 3
};
​
const reducedArr = _.reduce( arr, function ( sum, value, index, list ) {
  return sum, value;
}, 0 );
console.log( "Original Object: ", obj );
console.log( "Reduced Object: ", reducedArr );
```

#### \__.find_ <a id="_-find"></a>

* Iterates through each thing in the passed in collection and returns true in the passed in function.
* **\_.find\( collection, iteratee\_function \);**
* The iteratee function receives a current value as a parameter

```javascript
// "FIND WITH ARRAY"
const arr = [ 1, 2, 3, 4, 5, 6 ];
​
const firstEven = _.find( arr, function ( num ) {
  return num % 2 === 0;
});
console.log( "Original Array: ", arr );
console.log( "First Even in the Array: ", firstEven );
​
// "FIND WITH OBJECT"
const obj = {
  one: 1,
  two: 2,
  three: 3
};
​
const firstEven = _.find( obj, function ( num ) {
  return num % 2 === 0;
});
console.log( "Original Object: ", arr );
console.log( "First Even Value in the Object: ", firstEven );
```

#### \__.filter_ <a id="_-filter"></a>

* Iterates through each thing in the passed in collection and returns everything that returns true in the passed in function.
* **\_.filter\( collection, iteratee\_function \);**
* The iteratee function receives a current value as a parameter

```javascript
// "FILTER WITH ARRAY"
const arr = [ 1, 2, 3, 4, 5, 6 ];
​
const allEvens = _.filter( arr, function ( num ) {
  return num % 2 === 0;
});
console.log( "Original Array: ", arr );
console.log( "All Evens in the Array: ", allEvens );
​
// "FILTER WITH OBJECT"
const obj = {
  one: 1,
  two: 2,
  three: 3,
  four: 4
};
​
const allEvens = _.filter( obj, function ( num ) {
  return num % 2 === 0;
});
console.log( "Original Array: ", obj );
console.log( "All Evens in the Array: ", allEvens );
```

#### \__.where_ <a id="_-where"></a>

* Iterates through each thing in the passed in collection and returns everything that has the same key and values
* **\_.where\( collection, object \);**

```javascript
// "WHERE WITH ARRAY FILLED WITH OBJECTS"
const books = [
  { author: "Gustave Flaubert", title: "Sentimental Education" },
  { author: "Marie-Henri Beyle", title: "Lucien Leuwen" }
];
​
const gustave = _.where( books, { author: "Gustave Flaubert" } );
console.log( "Original Books Array: ", books );
console.log( "Gustave Flaubert's Books: ", gustave );
​
// Note - _.findWhere is the same as .find, but returns the first object only. It's 
// really handy for IDs or other values in a collection that you know will be unique.
```

#### \__.reject_ <a id="_-reject"></a>

* Iterates through each thing in the passed in collection and removes everything that returns true in the passed in function.
* **\_.reject\( collection, iteratee\_function \);**
* The iteratee function receives a current value as a parameter

```javascript
// "REJECT WITH ARRAY"
const arr = [ 1, 2, 3, 4, 5, 6 ];
​
const odds = _.reject( arr, function ( num ) {
  return num % 2 === 0;
});
console.log( "Original Array: ", arr );
console.log( "Odds Array: ", odds );
```

#### \__.contains_ <a id="_-contains"></a>

* Iterates through each thing in the passed in collection and returns true if it has the passed in value
* **\_.contain\( collection, value \);**

```javascript
// "CONTAIN WITH ARRAY"
const arr = [ 1, 2, 3 ];
​
const containsThree = _.contains( arr, 3 );
console.log( "Original Array: ", arr );
console.log( "It contains three? ", containsThree );
```

#### \__.pluck_ <a id="_-pluck"></a>

* Iterates through each thing in the passed in collection and returns just the requested key
* **\_.pluck\( collection, key \);**

```javascript
// "PLUCK WITH ARRAY OF OBJECTS"
const books = [
  { author: "Gustave Flaubert", title: "Sentimental Education" },
  { author: "Marie-Henri Beyle", title: "Lucien Leuwen" }
];
​
const authors = _.pluck( books, 'author' );
console.log( "Original Books Array: ", books );
console.log( "All Authors: ", authors );
```

#### \__.max_ <a id="_-max"></a>

* Returns the maximum value in the array
* **\_.max\( collection \);**

```javascript
// "MAX WITH ARRAY"
const arr = [ 1, 292898, 4, 232.223 ];
​
const maxNum = _.max( arr );
console.log( "Original Array: ", arr );
console.log( "Maximum Number: ", maxNum );
​
// "MAX WITH OBJECT"
const people = [
  { name: "Marcel", age: Infinity },
  { name: "Roger", age: 34 }
];
​
const oldestPerson = _.max( people, function ( person ) {
  return person.age;
});
console.log( "Original Array: ", people );
console.log( "Oldest Person: ", oldestPerson );
```

#### \__.min_ <a id="_-min"></a>

* Returns the minimum value in the array
* **\_.min\( collection \);**

```javascript
// "MIN WITH ARRAY"
const arr = [ 0.1, 292898, 4, 232.223 ];
​
const maxNum = _.min( arr );
console.log( "Original Array: ", arr );
console.log( "Minimum Number: ", maxNum );
​
// "MIN WITH OBJECT"
const people = [
  { name: "Marcel", age: Infinity },
  { name: "Roger", age: 34 }
];
​
const youngestPerson = _.min( people, function ( person ) {
  return person.age;
});
console.log( "Original Array: ", people );
console.log( "Youngest Person: ", youngestPerson );
```

#### \__.sortBy_ <a id="_-sortby"></a>

* Iterates through each item in the collection and sorts them using the given function
* **\_.sortBy\( collection, iteratee\_function \)**

```javascript
// "SORTBY WITH ARRAY"
const arr = [ 0.1, 292898, 4, 232.223 ];
const sortedArray = _.sortBy( arr, function (num) {
  return num;
});
console.log( "Original Array: ", arr );
console.log( "Sorted Array: ", sortedArray );
​
// "SORTBY WITH OBJECT"
const people = [
  { name: "Marcel", age: Infinity },
  { name: "Roger", age: 34 }
];
​
const youngestPerson = _.sortBy( people, "age" );
console.log( "Original Array: ", people );
console.log( "Sorted by Age: ", youngestPerson );
```

#### \__.shuffle_ <a id="_-shuffle"></a>

* Shuffles the given collection
* **\_.shuffle\( collection \);**

```javascript
// "SHUFFLE WITH ARRAY"
const arr = [ 1, 2, 3, 4, 5, 6 ];
const shuffledArr = _.shuffle( arr );
console.log( "Original Array: ", arr );
console.log( "Shuffled Array: ", shuffledArr );
```

#### \__.sample_ <a id="_-sample"></a>

* Picks a number \(default 1\) of elements from the given collection
* **\_.sample\( collection, to\_return \);**

```javascript
// "SAMPLE WITH ARRAY"
const arr = [ 1, 2, 3, 4, 5, 6 ];
const sample = _.sample( arr );
const threeSample = _.sample( arr, 3 );
console.log( "Original Array: ", arr );
console.log( "Sample: ", sample );
console.log( "Three Sampled: ", threeSample );
```

### Common methods - arrays <a id="common-methods-arrays"></a>

#### \__.first_ <a id="_-first"></a>

* Returns the first number of element\(s\) in the collection
* **\_.first\( collection, to\_pick \);**

```javascript
// "FIRST WITH ARRAY"
const arr = [ 1, 2, 3, 4, 5 ];
const firstOne = _.first( arr );
const firstThree = _.first( arr, 3 );
console.log( "Original Array: ", arr );
console.log( "First One: ", firstOne );
console.log( "First Three: ", firstThree );
```

#### \__.last_ <a id="_-last"></a>

* Returns the last number of element\(s\) in the collection
* **\_.last\( collection, to\_pick \);**

```javascript
// "LAST WITH ARRAY"
const arr = [ 1, 2, 3, 4, 5 ];
const lastOne = _.last( arr );
const lastThree = _.last( arr, 3 );
console.log( "Original Array: ", arr );
console.log( "Last One: ", lastOne );
console.log( "Last Three: ", lastThree );
```

#### \__.compact_ <a id="_-compact"></a>

* Removes all falsey values in an array
* **\_.compact\( collection \);**

```javascript
// "COMPACT WITH ARRAY"
const arr = [ 0, 1, false, 2, '', 3, undefined, NaN ];
const compactedArray = _.compact( arr );
console.log( "Original Array: ", arr );
console.log( "Compacted Array: ", compactedArray );
```

#### \__.flatten_ <a id="_-flatten"></a>

* Turns an array of arrays into one flat array \( can be specified to just flatten to one level \)
* **\_.flatten\( arr, flatten\_to\_one\_level\_true\_or\_false \);**

```javascript
// "FLATTEN WITH ARRAY"
const arr = [ [1], [2], [[[1]]] ];
const flattenedArray = _.flatten( arr );
const flattenedArrayToOneLevel = _.flatten( arr, true );
console.log( "Original Array: ", arr );
console.log( "Flattened Array: ", flattenedArray );
console.log( "Flattened Array (to one level): ", flattenedArrayToOneLevel );
```

#### \__.flatten_ <a id="_-flatten-1"></a>

* Returns the array without the specified pieces
* **\_.without\( collection, remove\_this, remove\_this... \);**

```javascript
// "WITHOUT WITH ARRAY"
const arr = [ 1, 2, 1, 0, 3, 1, 4 ];
const withoutOnes = _.without( arr, 1 );
const withoutOnesAndTwos = _.without( arr, 1, 2 );
console.log( "Original Array: ", arr );
console.log( "Without Ones Array: ", withoutOnes );
console.log( "Without Ones and Twos Array: ", withoutOnesAndTwos );
```

#### \__.union_ <a id="_-union"></a>

* Returns unique values from all given arrays in an array
* **\_.union\( collection, collection, collection \);**

```javascript
// "UNION WITH ARRAY"
const arr1 = [ 1, 2, 3 ];
const arr2 = [ 101, 22, 303.2 ];
const arr3 = [ 1, 2 ];
const uniqItems = _.union( arr1, arr2, arr3 );
console.log( "Array 1: ", arr1, " Array 2: ", arr2, " Array 3: ", arr3 );
console.log( "Unique Items: ", uniqItems );
```

#### \__.intersection_ <a id="_-intersection"></a>

* Returns values that are present in all given arrays as an array
* **\_.intersection\( collection, collection, collection \);**

```javascript
// "INTERSECTION WITH ARRAY"
const arr1 = [ 1, 2, 3 ];
const arr2 = [ 101, 2, 1, 10 ];
const arr3 = [ 2, 1 ];
const intersectedItems = _.intersection( arr1, arr2, arr3 );
console.log( "Array 1: ", arr1, " Array 2: ", arr2, " Array 3: ", arr3 );
console.log( "Items in all the Arrays: ", intersectedItems );
```

#### \__.uniq_ <a id="_-uniq"></a>

* Returns just unique values from the given array
* **\_.uniq\( arr \);**

```javascript
// "UNIQ WITH ARRAY"
const arr = [ 1, 2, 1, 4, 1, 3 ];
const uniqueItems = _.uniq( arr );
console.log( "Original Array: ", arr );
console.log( "Unique Items in Array Above: ", uniqueItems );
```

#### \__.zip_ <a id="_-zip"></a>

* Creates an array of arrays but changes the placement of the elements. Will make an array of the first elements, then all the second elements etc. Will return undefined if there are more elements in the first array
* **\_.zip\( first\_array, other\_array, other\_array \);**

```javascript
// "ZIP WITH ARRAYS"
const arr1 = [ 'moe', 'larry', 'curly' ];
const arr2 = [ 30, 40, 50 ];
const arr3 = [ true, false, false ];
const zippedArrays = _.zip( arr1, arr2, arr3 );
console.log( "Array 1: ", arr1, " Array 2: ", arr2, " Array 3: ", arr3 );
console.log( "Zipped Arrays: ", zippedArrays );
```

#### \__.object_ <a id="_-object"></a>

* Makes an object with the key coming from the first array and the value coming from the second array. Keeps going like that
* **\_.object\( arr1, arr2 \);**

```javascript
// "OBJECT WITH ARRAYS"
const arr1 = [ "moe", "larry", "curly" ];
const arr2 = [ 30, 40, 50 ];
const createdObject = _.object( arr1, arr2 );
console.log( "Array 1: ", arr1, " Array 2: ", arr2 );
console.log( "Created Object: ", createdObject );
```

#### \__.indexOf_ <a id="_-indexof"></a>

* Returns the index of the specified value. Will return -1 if it isn't present
*  **\_.indexOf\( collection, target \);**

```javascript
// "INDEXOF WITH ARRAYS"
const target = 2;
const arr = [ 1, 2, 3 ];
const indexOfTarget = _.indexOf( arr, target );
console.log( "Target to find index of: ", target, " Array: ", arr );
console.log( "Index of Target: ", indexOfTarget );
```

#### \__.range_ <a id="_-range"></a>

* Creates an array using a range
* **\_.range\( starting\_value, ending\_value, step \);**

```javascript
// "RANGE WITH ARRAYS"
console.log( "Passing in 10 to range: ", _.range( 10 ) );
console.log( "Passing in 10 and 20 to range: ", _.range( 10, 20 ) );
console.log( "Passing in -1, -11, and -1 to range: ", _.range( -1, -11, -1 ) );
```

### Common methods - objects <a id="common-methods-objects"></a>

#### \__.keys_ <a id="_-keys"></a>

* Returns an array of keys
* **\_.keys\( collection \);**

```javascript
// "KEYS WITH OBJECTS"
const obj = {
  one: 1,
  two: 2,
  six: 6
};
​
const objectKeys = _.keys( obj );
console.log( "Original Object: ", obj );
console.log( "Object Keys: ", objectKeys );
```

#### \__.values_ <a id="_-values"></a>

* Returns an array of values
* **\_.values\( collection \);**

```javascript
// "VALUES WITH OBJECTS"
const obj = {
  one: 1,
  two: 2,
  six: 6
};
​
const objectValues = _.values( obj );
console.log( "Original Object: ", obj );
console.log( "Object Values: ", objectValues );
```

#### \__.pairs_ <a id="_-pairs"></a>

* Returns an array of arrays with the key being the first element and the value being the second element
* **\_.pairs\( collection \);**

```javascript
// "PAIRS WITH OBJECTS"
const obj = {
  one: 1,
  two: 2,
  six: 6
};
​
const objectKeys = _.pairs( obj );
console.log( "Original Object: ", obj );
console.log( "Object Pairs in Array Form: ", objectKeys );
```

#### \__.invert_ <a id="_-invert"></a>

* Returns the opposite object. Keys become values
* **\_.invert\( collection \);**

```javascript
// "PAIRS WITH OBJECTS"
const obj = {
  one: 1,
  two: 2,
  six: 6
};
​
const objectKeys = _.invert( obj );
console.log( "Original Object: ", obj );
console.log( "Inverted Object: ", objectKeys );
```

#### \__.pick_ <a id="_-pick"></a>

* Returns an object with just the passed in keys \(white listing\)
* **\_.pick\( collection, key\_to\_keep, key\_to\_keep \);**

```javascript
// "PICK WITH OBJECTS"
const obj = {
  name: "Moe",
  age: 50,
  userID: 142423
};
​
const whiteListedObject = _.pick( obj, "name", "age" );
const functionWhiteListedObject = _.pick( obj, function ( value, key, object ) {
  return _.isNumber( value ); // Returns true if it is a number
} );
console.log( "Original Object: ", obj );
console.log( "White Listed Object (passing in keys): ", whiteListedObject );
console.log( "White Listed Object (passing in a function that returns a boolean): ", functionWhiteListedObject );
```

#### \__.omit_ <a id="_-omit"></a>

* Returns an object without the passed in keys \(black listing\)
* **\_.omit\( collection, key\_to\_remove, key\_to\_remove \);**

```javascript
// "OMIT WITH OBJECTS"
const obj = {
  name: "Moe",
  age: 50,
  userID: 142423
};
​
const omittedKeys = _.omit( obj, 'name' );
const omittedKeysWithFunction = _.omit( obj, function ( value, key, object ) {
  return _.isNumber( value );
} );
console.log( "Original Object: ", obj );
console.log( "Omitted Keys (specified with key): ", omittedKeys );
console.log( "Omitted Keys (specified by a function): ", omittedKeysWithFunction );
```

#### \__.has_ <a id="_-has"></a>

* Returns true if the object has the specified key
* **\_.has\( collection, key \);**

```javascript
// "HAS WITH OBJECTS"
const obj = {
  name: "Moe",
  age: 50,
  userID: 142423
};
​
const hasName = _.has( obj, "name" );
console.log( "Original Object: ", obj );
console.log( "Did it have the name key? ", hasName );
```

### Common functions <a id="common-functions"></a>

#### \__.delay_ <a id="_-delay"></a>

* Calls a function after a specified amount of time
*  **\_.delay\( function, time\_in\_ms \);**

```javascript
// "DELAY WITH FUNCTION"
const toCall = function () {
  console.log( "Delayed function Called." )
}
_.delay( toCall, 1000 );
```

#### \__.throttle_ <a id="_-throttle"></a>

* Says that the specified function can be called only every so often \(will call it straight away when it reads this line\). Throttle guarantees that the given function actually runs
* **\_.throttle\( function, time\_in\_ms \);**

```javascript
// "THROTTLE WITH FUNCTION"
const showScrollTop = function () {
  const scrollTop = $( window ).scrollTop();
  console.log( "Scroll Top is: ", scrollTop );
}
const throttledShowScrollTop = _.throttle( showScrollTop, 100 );
$( window ).scroll( throttledShowScrollTop );
// This function can only call every 100 milliseconds but will call straight away 
// when defined
```

#### \__.debounce_ <a id="_-debounce"></a>

* Says that the specified function can be called only every so often \(won't call straight away\)
* **\_.debounce\( function, time\_in\_ms \);**

```javascript
// "DEBOUNCE WITH FUNCTION" );
const calculateLayout = function () {
  const windowWidth = $( window ).width();
  console.log( "Window Width is: ", windowWidth );
}
const debouncedCalculateLayout = _.debounce( calculateLayout, 300 );
$(window).resize( debouncedCalculateLayout );
// This function can only call every 300 ms but won't call straight away
```

#### \__.once_ <a id="_-once"></a>

* Says that the specified function can be called only once
* **\_.once\( function \);**

```javascript
// "ONCE WITH FUNCTION"
const createApplication = function () {
  console.log( "Create Application called." );
}
const initialize = _.once( createApplication );
initialize(); // This will call
initialize(); // This won't call
```

#### \__.times_ <a id="_-times"></a>

* Call the passed in function a specified amount of times \(receives an index as a parameter\)
* **\_.times\( num\_of\_times, function \);**

```javascript
// "TIMES WITH FUNCTION"
_.times( 3, function ( index ) {
  console.log( "Index: ", index );
} );
```

#### \__.random_ <a id="_-random"></a>

* Generates a random value between 0 and the passed in number if just one value is passed in. Or between the first and second values. Best to be explicit and pass in 0 if necessary
* **\_.random\( starting\_point, ending\_point \);**

```javascript
// "RANDOM WITH FUNCTION"
const upToOneHundred = _.random( 100 );
const betweenOneHundredAndTwoHundred = _.random( 100, 200 );
const betweenOneHundredAndMinusTwoHundred = _.random( 100, -200 );
const betweenMinusOneHundredAndMinusTwoHundred = _.random( -100, -200 );
console.log( "Up to 100: ", upToOneHundred );
console.log( "Between 100 and 200: ", betweenOneHundredAndTwoHundred );
console.log( "Between 100 and -200: ", betweenOneHundredAndMinusTwoHundred );
console.log( "Between -100 and -200: ", betweenMinusOneHundredAndMinusTwoHundred );
```

### Predicate methods <a id="predicate-methods"></a>

#### \__.isEqual_ <a id="_-isequal"></a>

* Checks whether two collections are equal
* **\_.isEqual\( collection\_one, collection\_two \);**

```javascript
// "ISEQUAL WITH COLLECTION"
const arr1 = [ 0, 1, 2 ];
const arr2 = [ 0, 1, 2 ];
const returnedEquals = arr1 === arr2; // Returns false
const returned = _.isEqual( arr1, arr2 ); // Returns true
console.log( "Thing 1: ", arr1, " Thing 2: ", arr2 );
console.log( "Thing 1 and Thing 2 compared with three equals: ", returnedEquals );
console.log( "Thing 1 and Thing 2 compared with isEqual: ", returned );
```

#### \__.isMatch_ <a id="_-ismatch"></a>

* Tells you if the keys and values in properties are contained in object.
* **\_.isMatch\( collection, obj \);**

```javascript
//  "ISMATCH WITH OBJECT"
const obj = {
  name: "Roger"
};
const matched = _.isMatch( obj, { name: "Roger" } ); // Returns true
console.log( "Object: ", obj );
console.log( "Object has a name: ", matched );
```

#### \__.isEmpty_ <a id="_-isempty"></a>

* Returns true if there is nothing in the array or the object
* **\_.isEmpty\( thing \);**

```javascript
// "ISEMPTY WITH COLLECTION"
const emptyArr = [];
const notEmptyObj = {
  name: "Roger"
};
const emptyArrMethod = _.isEmpty( emptyArr ); // Returns true
const filledObjMethod = _.isEmpty( notEmptyObj ); // Returns false
console.log( "Array is: ", emptyArr, ". Is it empty? ", emptyArrMethod );
console.log( "Object is: ", notEmptyObj, ". Is it empty? ", filledObjMethod );
```

#### \__.isArray_ <a id="_-isarray"></a>

* Returns true if it is an array
* **\_.isArray\( thing \);**

```javascript
// "ISARRAY WITH THING"
const arr = [];
const obj = {};
const arrMethod = _.isArray( arr ); // Returns true
const objMethod = _.isArray( obj ); // Returns false
console.log( "Thing 1: ", arr, ". Is it an array? ", arrMethod );
console.log( "Thing 2: ", obj, ". Is it an array? ", objMethod );
```

#### \__.isObject_ <a id="_-isobject"></a>

* Returns true if it is an object
* **\_.isObject\( thing \);**

```javascript
// " ISOBJECT WITH THING"
const arr = [];
const obj = {};
const arrMethod = _.isObject( arr ); // Returns false
const objMethod = _.isObject( obj ); // Returns true
console.log( "Thing 1: ", arr, ". Is it an object? ", arrMethod );
console.log( "Thing 2: ", obj, ". Is it an object? ", objMethod );
```

#### \__.isFunction_ <a id="_-isfunction"></a>

* Returns true if it is a function
* **\_.isFunction\( thing \);**

```javascript
// "ISFUNCTION WITH THING"
const myFunc = function () {};
const arr = [];
const funcMethod = _.isFunction( myFunc ); // Returns true
const arrMethod = _.isFunction( arr ); // Returns false
console.log( "Thing 1: ", myFunc, ". Is it a function? ", funcMethod );
console.log( "Thing 2: ", arr, ". Is it a function? ", arrMethod );
```

#### \__.isString_ <a id="_-isstring"></a>

* Returns true if it is a string
* **\_.isString\( thing \);**

```javascript
// "ISSTRING WITH THING"
const myFunc = function () {};
const str = "";
const funcMethod = _.isFunction( myFunc ); // Returns false
const strMethod = _.isFunction( str ); // Returns true
console.log( "Thing 1: ", myFunc, ". Is it a string? ", funcMethod );
console.log( "Thing 2: ", str, ". Is it a string? ", strMethod );

```

#### \__.isNumber_ <a id="_-isnumber"></a>

* Returns true if it is a number
* **\_.isNumber\( thing \);**

```javascript
// "ISNUMBER WITH THING"
const myFunc = function () {};
const num = 1;
const funcMethod = _.isNumber( myFunc ); // Returns false
const numMethod = _.isNumber( num ); // Returns true
console.log( "Thing 1: ", myFunc, ". Is it a number? ", funcMethod );
console.log( "Thing 2: ", num, ". Is it a number? ", numMethod );
```

#### \__.isFinite_ <a id="_-isfinite"></a>

* Returns true if it is a finite value
* **\_.isFinite\( thing \);**

```javascript
// "ISFINITE WITH COLLECTION"
const infiniteThing = -Infinity;
const finiteThing = 1;
const infiniteMethod = _.isFinite( infiniteThing ); // Returns false
const finiteMethod = _.isFinite( finiteThing ); // Returns true
console.log( "Thing 1: ", infiniteThing, ". Is it finite? ", infiniteMethod );
console.log( "Thing 2: ", finiteThing, ". Is it finite? ", finiteMethod );
```

#### \__.isBoolean_ <a id="_-isboolean"></a>

// Returns true if it is a boolean value // **\_.isBoolean\( thing \);**

```javascript
// "ISBOOLEAN WITH COLLECTION"
const isTrue = true;
const str = "";
const trueMethod = _.isBoolean( isTrue ); // Returns true
const strMethod = _.isBoolean( str ); // Returns false
console.log( "Thing 1: ", isTrue, ". Is it a function? ", trueMethod );
console.log( "Thing 2: ", str, ". Is it a function? ", strMethod );
```

#### \__.isNAN_ <a id="_-isnan"></a>

* Returns true if it is NaN

  -**\_.isNaN\( thing \);**

```javascript
// "ISNAN WITH COLLECTION"
const str = "";
const nope = NaN;
const strMethod = _.isNaN( str ); // Returns false
const nanMethod = _.isNaN( nope ); // Returns true
console.log( "Thing 1: ", str, ". Is it NaN? ", strMethod );
console.log( "Thing 2: ", nope, ". Is it NaN? ", nanMethod );
```

#### \__.isNull_ <a id="_-isnull"></a>

* Returns true if it is null
* **\_.isNull\( thing \);**

```javascript
// "ISNULL WITH COLLECTION"
const str = "";
const nope = null;
const strMethod = _.isNull( str ); // Returns false
const nullMethod = _.isNull( nope ); // Returns true
console.log( "Thing 1: ", str, ". Is it null? ", strMethod );
console.log( "Thing 2: ", nope, ". Is it null? ", nullMethod );
```

#### \__.isUndefined_ <a id="_-isundefined"></a>

* Returns true if it is undefined
* **\_.isUndefined\( thing \);**

```javascript
// "ISUNDEFINED WITH COLLECTION"
const str = "";
const nope = undefined;
const strMethod = _.isUndefined( str ); // Returns false
const undefinedMethod = _.isUndefined( nope ); // Returns true
console.log( "Thing 1: ", str, ". Is it undefined? ", strMethod );
console.log( "Thing 2: ", nope, ". Is it undefined? ", undefinedMethod );
```

### _JavaScript - Underscore - Exercises_ <a id="javascript-underscore-exercises"></a>

* ​[Collections I​](https://gist.github.com/textchimp/beab9f0d9eac9e347dcbcee37accc9ab)
* [​Collections II​](https://gist.github.com/textchimp/1f15e1bf2fb5a48e476f6b7ca4aac575)

## Flickr API w/ AJAX <a id="flickr-api-w-ajax"></a>

Set up your environment and create all required files.

```bash
mkdir Flickr
cd Flickr
touch index.html
mkdir js
touch js/jquery.js # remember that you will actually have to curl the jquery file with it's contents or create a script tag in your index.html linking the CDN
touch js/underscore.js
touch js/search.js
mkdir css
touch css/style.css
```

Set up form in index.html

* Use Emmet to autofill your page with `! + tab`

Add a script/link tag to the head to link both the `search.js` and `style.css`

```markup
  <script src="js/main.js" charset="utf-8"></script>
```

```markup
  <link rel="stylesheet" type="text/css" href="style.css">
```

```markup
<div class="container">
    <form>
        <input type="search" id="query">
```

## Homework

Build a single page app that displays data from an API using AJAX

### Resources

* [Nasa API](https://api.nasa.gov/)
* [Wolfram alpha API](http://products.wolframalpha.com/api/)
* [Pokemon API](http://pokeapi.co/)
* [Weather API](https://openweathermap.org/api)
* [Marvel API](https://developer.marvel.com/)
* [Aus census API](https://www.data.gov.au/)
* [Other open APIs](https://github.com/toddmotto/public-apis)
* [Wiki list of open APIs](https://en.wikipedia.org/wiki/List_of_open_APIs)
* [The open web list of APIs](https://www.programmableweb.com/category/all/apis)

Further reading:

* [AJAX resource](https://gist.github.com/wofockham/3d4faf65dc9a5674ce11)s



