---
description: '$(''document'').readyForTheWeekendAmIRight(function(){});'
---

# Day 05

What we covered today:

* JavaScript - jQuery - Events
* JavaScript - jQuery - Animation and Effects
* JavaScript - jQuery - Patterns and Anti-patterns
* JavaScript - jQuery - Plugins

### Warmup

* [Space Age](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week02/day05_space_age)

### Exercises <a id="exercises"></a>

* [​​jQuery Plugins ](https://gist.github.com/wofockham/5474c8ae3f078756fab4)​

### Classwork

* [jQuery Exercises II](https://github.com/wofockham/wdi-30/tree/master/04-jquery)

## JavaScript - jQuery - Events <a id="javascript-jquery-events"></a>

### Listeners and handlers <a id="listeners-and-handlers"></a>

We can use the `.on()` method to add an event listener to the selected jQuery objects.

The first parameter accepted by the .on method is the event to listen for - the 'listener'.

The last parameter accepted by the .on\(\) method is the 'handler' - the function to execute when the event is triggered. The handler can be either an anonymous function \(ie, the handler is the function itself\) or can be a call to another function.

```javascript
const onButtonClick = function() {
  console.log('clicked!');
};

$('button').on('click', onButtonClick); // Attaching an event handler with a defined function to the button

$('button').on('click', function () { // Attaching an event handler with an anonymous function to the button
  console.log('clicked!');
});

$('button').click(onButtonClick);
```

#### _Other event types_ <a id="other-event-types"></a>

* Keyboard Events - '`keydown`', '`keypress`', '`keyup`'
* Mouse Events - '`click`', '`mousedown`', '`mouseup`', '`mousemove`'
* Form Events - '`change`', '`focus`', '`blur`'

Arguments get passed into callbacks by default.

```javascript
const myCallback = function (event) {
    console.log( event );
    // The event parameter is a big object filled with ridiculous amounts of details about when the event occurred etc.
};

$('p').on('mouseenter', myCallback);

```

#### _Preventing default events_ <a id="preventing-default-events"></a>

```javascript
$('a').on('click', function (event) {
    event.preventDefault();
    console.log('Not going there!');
});

$('form').on('submit', function (event) {
    event.preventDefault();
    console.log('Not submitting, time to validate!');
});
```

## JavaScript - jQuery - Animation and Effects <a id="javascript-jquery-animation-and-effects"></a>

jQuery give us access to a whole bunch of really useful methods for creating and controlling animations and effects. These include simple methods for frequently used effects like .fadeIn and .fadeOut, as well as more complex and customizable methods like .animate. For a full list, check out [jQuery.com - Effects](http://api.jquery.com/category/effects/).

A few common ones are:

* `toggle`
* `fadeIn`
* `fadeOut`
* `show`
* `hide`
* `animate`

```javascript
// TOGGLE - changes the selected element's display property to show or hide (display: none) the element depending on the value of the element's display property when the method is called. This effectively calls the .show or .hide methods, depending on whether the element is display: none or display: block/inline-block/inline/table when the method is called.
$('#error').toggle();

// The .toggle method takes two optional arguments - the duration (of the transition to/from display:none) and a function to call once the animation is complete. This can be a named function or an anonymous function (the example below calls an anonymous function):

$("#error").toggle(1000, function() {
  alert("ERROR");
});

// FADEIN - displays an element by changing its opacity to 1 (ie, opaque). If no duration is passed in as an argument, the default duration of 400ms will apply
$('#error').fadeIn();


// As with most of jQuery's effects, the .fadeIn method takes two optional arguments - the duration (of the transition to opacity: 1) and a function to call once the animation is complete. This can be a named function or an anonymous function (the example below calls a named function):

$("#error").fadeIn(1000, errorFunction);

//  SHOW - Show the element by changing its display property from display:none.
$('#error').show(1000, function(){
    $(this).addClass('redText')
});

//  HIDE - Hide the element by changing its display property to display: none.
$('#error').hide(1000, function(){
    $(this).addClass('removed')
});

// ANIMATE - Allows us to animate a custom set of CSS properties. Remember that a dash-cased CSS property name (eg font-size) must be camelCased for JavaScript's benefit (eg fontSize). We can only change properties that can be reduced to numeric values, so the core jQuery library doesn't allow animation of, for example, background-color (some plugins and extensions, like jQuery UI, can achieve this).
$("#cat").animate({
  width: "500px",
  fontSize: "24px",
  letterSpacing: "2px",
  marginLeft: "20px"
}, 200)
```

## JavaScript - jQuery - Patterns and anti-patterns <a id="javascript-jquery-patterns-and-anti-patterns"></a>

#### _The_ `ready` _event_ <a id="the-ready-event"></a>

In order to do cool jQuery stuff, we need to make sure that all of the content in the HTML document has been parsed by the browser before our script is run, so put all your DOM-related jQuery code in a function that is called when the `document` is `ready`.

```javascript
$(document).ready(function(){

});
```

The below function works in a similar way the the `ready` function but will wait until ALL elements \(eg. images\) are loaded before it will run the code block.

```javascript
$(document).load(function(){

});
```

#### _Variable names_ <a id="variable-names"></a>

Pattern: Prefix variable names that represent jQuery objects with `$`. That will remind us that this object has all of jQuery's fancy methods available to it.

Remember: in variable names, $ is just another character we can use. It's not magic!

```javascript
const $myVar = $('#myNode');
```

#### _Event delegation_ <a id="event-delegation"></a>

When elements are being created after the document is loaded, bind the event to an element that will _definitely_ exist on document.ready \(a failsafe element would be `$(document)`\).

```javascript
$("body").on('click', 'a', myCallback);
```

#### _Named functions for event handlers_ <a id="named-functions-for-event-handlers"></a>

Event handlers can either call anonymous functions or named functions. Anonymous functions are useful, but named functions are re-usable.

```javascript
// Anonymous function:
$p.on("click", function() {
$(this).css("backgroundColor", "gainsboro");
});

// Named function:  
const myCallback = function() {
  $(this).css("backgroundColor", "gainsboro");
};

$("p").on('click', myCallback);
```

#### _Chaining methods_ <a id="chaining-methods"></a>

jQuery lets us chain methods together, allowing for pretty succinct code.

```javascript
banner.css('color', 'red');
banner.html('Welcome!');
banner.show();

// Is the same as:
banner.css('color', 'red').html('Welcome!').show();

// Is the same as:
banner.css('color', 'red')
      .html('Welcome!')
      .show();
```

## JavaScript - jQuery - Plugins <a id="javascript-jquery-plugins"></a>

### Finding plugins <a id="finding-plugins"></a>

Go through

* ​[jQuery plugin](http://plugins.jquery.com/)​
* ​[here](https://www.javascripting.com/)​

### Selecting a good plugin <a id="selecting-a-good-plugin"></a>

Look for plugins that:

* Have good documentation
* Are flexible
* Have some community around them
  * Check the number of forks and issues on GitHub.
* Are actively maintained
  * When was the last commit in the GitHub repo?
* Have a small file size
* Have good browser compatibility
* Have responsive design

For more details, go [here.](http://blog.pamelafox.org/2013/07/which-javascript-library-should-i-pick.html)​

### Using a plugin <a id="using-a-plugin"></a>

* Download the plugin and associated files from the site or Github repository
* Copy the files into your project's folder
* In the HTML, reference any associated CSS
* In your HTML, after you've referenced jQuery, and before you reference your own code, add the script tag\(s\) that reference the plugins JS file\(s\).

## Homework <a id="homework"></a>

* ​GA Bank​
* ​[Try jQuery](http://try.jquery.com/)​
* ​[Learn jQuery](https://learn.jquery.com/)​
* ​[jQuery API](https://api.jquery.com/)​

