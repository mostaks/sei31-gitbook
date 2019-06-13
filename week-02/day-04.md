---
description: The almighty $
---

# Day 04

What we covered today:

* JavaScript - Libraries
* JavaScript - jQuery - Introduction

### Warmup

* [99 Bottles of Beer](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week02/day04_99_bottles)

### Slides <a id="slides"></a>

* ​[jQuery - Introduction​​](https://www.teaching-materials.org/_deprecated/jquery/#/)

### Exercises <a id="exercises"></a>

1. ​[Making a Video Player​](https://gist.github.com/wofockham/0699f2bbe156ad25ad4a)
2. [Making a Video Player - jQuery](https://gist.github.com/wofockham/772d8687f02dad503469)
3. ​ [jQuery Events & Animation](https://gist.github.com/wofockham/a834abbcf9b3c542a1c3)

### Classwork

* [jQuery Exercises](https://github.com/wofockham/wdi-30/tree/master/04-jquery)

### Links <a id="links"></a>

​[Programmer Ryan Gosling](http://programmerryangosling.tumblr.com/)​

## JavaScript - Libraries <a id="javascript-libraries"></a>

### What is a library? <a id="what-is-a-library"></a>

A collection of reusable methods designed for a particular purpose. You just reference a javascript file with a particular library in it - and away you go!

jQuery is the most common JavaScript library on the web today. As of 2016, it was used in:

* Over 43 million websites, including:
  * Over 12% of all websites;
  * Over 78% of the top million websites;

## JavaScript - jQuery - Introduction <a id="javascript-jquery-introduction"></a>

#### _What is jQuery_ <a id="what-is-jquery"></a>

An open source JavaScript library that simplifies the interaction between HTML and JavaScript. A JavaScript library is collection of reusable methods for a particular purpose.

It was created by John Resig in 2005, and released in January of 2006.

Built in an attempt to simplify the existing DOM APIs and abstract away cross-browser issues.

#### _Why jQuery?_ <a id="why-jquery"></a>

* Well documented
* Lots of plugins
* Small-ish size \(23kb\)
* Everything works in IE 6+, Firefox 2+, Safari 3+, Chrome, and Opera 9+ \(if you are using 1.11.3 or previous, greater than that scraps up until IE8 support\)
* Millions and millions of sites using it.

#### _What does it do for us?_ <a id="what-does-it-do-for-us"></a>

* Data Manipulation
* DOM Manipulation
* Events
* AJAX
* Effects and Animation
* HTML Templating
* Widgets / Theming
* Graphics / Chart
* App Architecture

#### _Why use it?_ <a id="why-use-it"></a>

```javascript
// No library:
const elems = document.getElementsByTagName("img");
for (let i = 0; i< elems.length; i++) {  
  elems[i].style.display = "none";
}​
​
// jQuery:
$('img').hide();
```

> "The code that has the least amount of bugs is the code that doesn't exist." - _Joel Turnbull_

### The basics <a id="the-basics"></a>

**Select -&gt; Manipulate -&gt; Admire**

```javascript
const $paragraphs = $("p")
$paragraphs.addClass('special');​
​
// OR
$("p").addClass("special");
```

#### _Selecting elements_ <a id="selecting-elements"></a>

All CSS selectors are valid.

| HTML | CSS Selector | jQuery |
| :--- | :--- | :--- |
| `<p>` | `p` | `$('p')` |
| `<p class="intro">` | `.intro` | `$('.intro')` |
| _or_ | `p.intro` | `$('p.intro')` |
| `<p id="badger">` | `#badger` | `$('#badger')` |
| `<div> <p></p> </div>` | `div p` | `$('div p')` |
| `<ul> <li></li> <li></li> </ul>` | `ul li:last-child` | `$('ul li:last-child')` |

jQuery also offers a bunch of other selectors. Some common ones are:

| Selector | Example | Selects | ​ | ​ |
| :--- | :--- | :--- | :--- | :--- |
| `:first` | `$("p:first")` | The first paragraph element. | ​ | ​ |
| `:last` | `$("div > p:last")` | The last paragraph element that is a direct child of a div element | ​ | ​ |
| `:has()` | `$("div:has(img)")` | Any divs that have one or more descendent img elements | ​ | ​ |
| `:visible` | `$(".kitten:visible")` | Any elements with the class "kitten" that have height | ​ | width &gt; 0 |
| `:hidden` | `$(".kitten:hidden")` | The opposite of :visible. Includes `display:none`. | ​ | ​ |

See [jQuery API - Selectors](http://api.jquery.com/category/selectors/).

#### _Reading elements_ <a id="reading-elements"></a>

If we had this element in the HTML...

`<a id="yahoo" href="http://www.yahoo.com" style="font-size:20px">Yahoo!</a>`

We can select it using `$("a#yahoo")`

We can store it using `let $myLink = $("#yahoo");`

We can get the content within it using `$("#yahoo").html()`

We can get the text within it using `$("#yahoo").text()`

We can get the HREF attribute using `$("#yahoo").attr("href")`

We can get the CSS attribute using `$("#yahoo").css('font-size')`

#### _Changing elements_ <a id="changing-elements"></a>

`$("#yahoo").attr("href", "http://generalassemb.ly")`

`$("#yahoo").css("font-size", "25px")`

`$("#yahoo").text("General Assembly")`

`$("#yahoo").attr("href", "http://generalassemb.ly").css("font-size", "25px").text("General Assembly")`

#### _Create, manipulate and inject_ <a id="create-manipulate-and-inject"></a>

```javascript
// Step 1: Create element and store a reference    
const $para = $('<p></p>'); // You can create any element with this!​
​
// Step 2: Use a method to manipulate (optional)    
$para.addClass('special'); // So many functions you could use
​
​// Step 3: Inject into your HTML    
$('body').append($para); // Also could use prepend, prependTo or appendTo as well
```

### HTML elements v DOM nodes v jQuery objects <a id="html-elements-v-dom-nodes-v-jquery-objects"></a>

A DOM node is not actually an HTML element - it is a representation of an HTML element, which we can use to create, modify, and read properties of HTML elements.

Similarly, it's important to note that a jQuery object is not actually a DOM node - it is a wrapper around a DOM element/collection of DOM elements that allows us to use the jQuery library's methods.

Consider this HTML:

```markup
<div>  
  <p> This is a paragraph of text.</p>
</div>
```

We can't access and interact with an HTML element directly from JavaScript.

```markup
<p>
// => Uncaught SyntaxError: Unexpected token <
```

We need to use the DOM API to select the DOM node that represents that HTML element.

```javascript
document.querySelector("p");
// => <p> This is a paragraph of text.</p>
```

This is a DOM node, not an HTML element.

```javascript
typeof(document.querySelector("p"));
// => "object"
```

If we create a jQuery object but want to access the DOM elements within that object, we need to pull out the native DOM element from the jQuery object.

```javascript
let paragraph = document.querySelector("p");  
// This is a DOM node.
let $paragraph = $("p:first");                
// This is a jQuery element.
​
​$paragraph === paragraph
// => false
// The first object is a jQuery object. The second object is a DOM node.
​
​$paragraph;
// => [ <p> This is a paragraph of text </p> ]
// Note the square brackets - this is a jQuery object, not a DOM node.
​
​let p = paragraph[0]
// We have taken the jQuery object and stored the first DOM node in that element to 
//a variable called p
​
​p === paragraph
// => true
```

While we can mix regular JavaScript and jQuery together in our code, we cannot call jQuery methods on regular DOM objects \(and vice-versa\).

```javascript
// CREATING A JQUERY OBJECT
​
​let $paragraphs = $('p');
// This creates an array-like jQuery object comprised of all the DOM elements in the 
//Document Object Model that represent paragraph tags.​
​
// GETTING A DOM NODE FROM A JQUERY OBJECT - METHOD 1​
let firstParagraph = $paragraphs[0];
// This is a DOM node. We have selected the first DOM node in the $paragraphs jQuery 
//object​
​
// GETTING A DOM NODE FROM A JQUERY OBJECT - METHOD 2​
let firstParagraph = $paragraphs.get(0);
// This is a DOM node. We have selected the first DOM node in the $paragraphs jQuery 
//object using jQuery's .get() method.
​
​let allParagraphs = $paragraphs.get();
// This is an array DOM nodes. We have created 
//an array of all the native DOM nodes in the $paragraphs jQuery object using 
//jQuery's .get() method without passing any arguments.​
​
// CREATING A JQUERY OBJECT FROM ANOTHER JQUERY OBJECT​
let $myParagraph = $( paragraphs[0] );
// This is a jQuery object.
​
​let $myParagraph = $paragraphs.el(0);
// This is a jQuery object - this is the preferred method.
​
​// We can also loop through our array...
for( let i = 0; i < paragraphs.length; i++ ) {   
  let element = paragraphs[i];   
  let paragraph = $(element);   
  paragraph.html(paragraph.html() + ' wowee!!!!');
};​
​
// Or use jQuery to do it - this is preferred
$paragraphs.each(function () {  
  let $this = $( this );  
  $this.html( $this.html() + " wowee!!!" );
});
```

## Homework <a id="homework"></a>

* ​To learn jQuery from these \(and other\) resources AND try redoing yesterdays exercises \(aboutme, booklist, calculator, madlibs\) in jQuery
  * [https://learn.jquery.com/](https://learn.jquery.com/) 
  * [http://api.jquery.com/](http://api.jquery.com/) 
  * [http://jqfundamentals.com/](http://jqfundamentals.com/)
  *  [https://www.codecademy.com/learn/learn-jquery](https://www.codecademy.com/learn/learn-jquery) 
  * [https://www.youtube.com/watch?v=hMxGhHNOkCU](https://www.youtube.com/watch?v=hMxGhHNOkCU) 
* [A good site](http://speakingjs.com/es5/ch01.html) to revise JS

