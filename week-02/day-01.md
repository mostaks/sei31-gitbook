---
description: '.ideal-job { stress: 0; richness: 100; }'
---

# Day 01

What we covered today:

* HTML
* CSS

### Warmup

* [Badger's Revenge](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week02/day01_badgers_revenge)

### Classwork

* [HTML Intro](https://github.com/wofockham/wdi-30/tree/master/02-html-css/html-intro)
* [CSS Intro](https://github.com/wofockham/wdi-30/tree/master/02-html-css/css-intro)

## HTML <a id="html"></a>

### A brief history of HTML <a id="a-brief-history-of-html"></a>

* ​[The first website ever](http://info.cern.ch/hypertext/WWW/)​
* ​[The History of Web Design](http://www.americommerce.com/blog/The-History-of-Web-Design-Infographic)​
* ​[The Evolution of the Web](http://www.evolutionoftheweb.com/) - a prettier, interactive visualisation of the evolution of the web.

### What is HTML? <a id="what-is-html"></a>

* **HTML** = Hyper Text Markup Language
* Currently at Version 5 \(HTML5\)
* Made up of tags \(usually pairs of opening and closing tags\), which in turn create elements

```markup
<!--    This whole thing is an element      -->
<tagname attribute="attribute_value"></tagname>​
​
<!-- A paragraph tag with two classes - "default-paragraph" and "another-class". -->
<!-- Could be selected in CSS using p.default-paragraph.another-class { }        -->
<p class="default-paragraph another-class">Some content in here</p>
```

### What does an HTML document need? <a id="what-does-an-html-document-need"></a>

```markup
<!DOCTYPE html> <!-- Always have this - it describes which version of HTML you are 
using -->
<html>    
  <head></head> <!-- This is the meta data of the page, generally invisible -->​​    
  
  <body></body> <!-- This is where the actual content goes -->
</html>
```

```markup
<html>    
  <head> 
    <!-- This will display the name of the website -->        
    <title>An Introduction to HTML</title>    
    <meta charset="utf-8">  <!-- Doesn't need an ending tag as there is no content -->
  </head> <!-- This is the meta data of the page, generally invisible -->    
  <body>​      
  
    <!-- This is where the content of your website will go -->​    
  </body> <!-- This is where the actual content goes -->
</html>
```

### Common HTML elements <a id="common-html-elements"></a>

#### _Actual content_ <a id="actual-content"></a>

```markup
<!-- Heading Tags - the lower the number, the more important -->
<h1></h1>
<h2></h2>
<h3></h3>
<h4></h4>
<h5></h5>
<h6></h6>
​
​<p></p> <!-- A paragraph tag -->​
​
<!-- An HTML element can have attributes.  Attributes are key value pairs 
(just like javascript objects) that provide additional information. 
They look like this. This is a link by the way (or anchor tag) -->
<a href="https://generalassemb.ly/" target="_blank">General Assembly</a>
<!-- target="_blank" will open the page in a new window, this should only be used for 
anchors linking to external websites -->​
​
<img src="" />
​
​<!-- you must use the direct link using 'https://{websiteDomain}/image.jpg' to link 
to the image -->​
​
<img src="https://naturecanada.ca/wp-content/uploads/2016/02/Monarch-1.jpg" />
​
​<!-- NOTE: make sure you're not 'hot linking' to the images as they can be replaced 
by the own to something a little more unsavoury -->
​
​<video src=""></video>
​
​<br /> <!-- a new line -->
<hr /> <!-- a horizontal line -->
​
​<button></button>
<input />​
​
<pre></pre> <!-- Preformatted text -->
<code></code>​
​
<textarea></textarea>
​
​<ul> <!-- Unordered list -->    
  <li></li> <!-- List item -->
</ul>​
​
<ol> <!-- Ordered list -->    
  <li></li>
</ol>
```

#### _Sectioning content_ <a id="sectioning-content"></a>

```markup
<div></div> <!-- A division, this is just a way to group content -->
<section></section>
<header></header>
<main></main>
<nav></nav>
<!-- etc. -->
```

### Placeholders <a id="placeholders"></a>

You'll be surprised how much time you spend generating placeholder text and images when designing a layout. Here are some tools to make that process a little less painful \(and, often, a little more fun\):

#### _Text_ <a id="text"></a>

* ​[Meet the Ipsums](http://meettheipsums.com/)​
* ​[Monocle Ipsum](http://www.monocleipsum.com/?paras=5&type=business-class&start-with-lorem=1)​
* ​[Hipster Ipsum](https://hipsum.co/)​
* ​[Samuel L. Jackson Ipsum](http://slipsum.com/)​
* ​[Wiki Ipsum](http://www.wikipsum.com/)​
* ​[Social Ipsum](http://socialgoodipsum.com/#!/#top)​
* ​[56 Other ones](http://mashable.com/2013/07/11/lorem-ipsum/)​

#### _Images_ <a id="images"></a>

* ​[Fill Murray](http://www.fillmurray.com/)​
* ​[Placeholdit](http://placehold.it/)​
* ​[Lorem pixel](http://lorempixel.com/)​
* ​[Place Bear](http://placebear.com/)​
* ​[Dummy Image](http://dummyimage.com/)​
* ​[Place Kitten](https://placekitten.com/)​
* ​[Place Cage](http://www.placecage.com/)​

### _HTML - Recommended Readings_ <a id="html-recommended-readings"></a>

* ​[MDN - HTML Element Reference](https://developer.mozilla.org/en/docs/Web/HTML/Element)​
* ​[MDN - HTML5 - Semantics](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5#SemantIcs)​
* ​[Let's Talk About Semantics](http://html5doctor.com/lets-talk-about-semantics)​
* ​[Jon Duckett - HTML and CSS](http://www.htmlandcssbook.com/)​

## CSS <a id="css"></a>

### What is CSS? <a id="what-is-css"></a>

* **CSS** = Cascading Style Sheets
* It defines how HTML elements are to be represented
* Styles were added to HTML4
* Currently at Version 3 \(CSS3\)

```css
  <!-- you will need to link your CSS file to your index.html file before you can do 
  anything -->  
  <link rel="stylesheet" href="css/intro.css">
```

```css
/*Syntax: */
selector {  /* CSS Rule - a selector and a set of declarations */    
  property: value; /* CSS Declaration - a [property: value] pair)*/
}​
​
/*Examples:*/
body {    
  background-color: limegreen;
}
​
​p {    
  font-style: italics;
}
​
​h1 {  
  text-decoration: underline;
}
```

### CSS selectors <a id="css-selectors"></a>

There are a lot of different selectors, but here are the basic CSS Selectors.

```css
p {} /* Selects all paragraph tags */
​
​p.intro {} /* Selects all paragraph tags with the class intro: <p class="intro>" */​
​
.intro {} /* Selects any tag with the class intro */
​
​p#intro {} /* Selects any paragraph tag with the ID intro */​
​
#intro {} /* Selects any element with the ID intro */​
​
* {} /* Selects all elements */​
​
div p {} /* Selects all paragraph tags that are within div tags */​
​
div > p {} /* Selects all the paragraph tags that are direct children of div tags */
​
​p, a {} /* Selects all p tags and all a tags */​
​
input[type="email"] /* Selects all inputs where the 'type' attribute has a value of 
'email' */​
​
/*Colons denote pseudo-classes, and allow us to select elements based on their state. 
These are really powerful. Check out the further reading below*/
​
p:hover {} /* Selects all p tags when they are hovered over */​
​
p:not(.intro) {} /* Selects all paragraph tags that do NOT have the class intro */​
​
/*Double-colons denote pseudo-elements, and allow us to style parts of the document . 
Check out the further readings below*/
p::after {} /* Creates a pseudo-element after every paragraph tag */
```

#### _CSS - CSS Selectors - Recommended Readings_ <a id="css-css-selectors-recommended-readings"></a>

* ​[MDN - CSS Selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors)​
* ​[CSS Diner](http://flukeout.github.io/) - a great way to get learn about selectors.
* ​[EnvatoTuts - 30 CSS Selectors You Must Memorize](http://code.tutsplus.com/tutorials/the-30-css-selectors-you-must-memorize--net-16048)​
* ​[MDN - CSS Pseudo-classes](https://developer.mozilla.org/en-US/docs/Web/CSS/pseudo-classes)​
* ​[MDN - CSS Pseudo-elements](https://developer.mozilla.org/en/docs/Web/CSS/Pseudo-elements)​

### CSS specificity <a id="css-specificity"></a>

Due to CSS's cascading nature, a CSS rule can be overwritten by subsequent, conflicting rules. This is subject to the rules of CSS specificity.

CSS specificity assigns 'points' or 'weight' to different CSS selectors. If two selectors select the same element, the selector with the most specificity points wins. If two selectors have equal specificity, the last rule will apply.

This high-level summary will get you through 9-10 of specificity issues:

* Inline styles always win \(but should generally be avoided\)
* Selectors using IDs come next
* Selectors using classes, attributes and pseudo-classes come next
* Selectors using element names come last
* Where the specificity of two selectors are identical, the last rule specified prevails

#### _CSS - CSS Specificity - Recommended Readings_ <a id="css-css-specificity-recommended-readings"></a>

* ​[Smashing Magazine - CSS Specificity](http://www.smashingmagazine.com/2007/07/27/css-specificity-things-you-should-know/)​
* ​[CSS-Tricks - Specifics on CSS Specificity](https://css-tricks.com/specifics-on-css-specificity/)​

### Variadic attributes <a id="variadic-attributes"></a>

Variadic attributes are shorthands that apply values to a number of properties - the properties modified will depend on the number of values specified in the declaration. Take `margin`, for example:

```css
h1 {    
  /* Applies to all four sides */    
  margin: 1em;​    
  
  /* vertical | horizontal */    
  margin: 5% auto;​    
  
  /* top | horizontal | bottom */    
  margin: 1em auto 2em;​    
  
  /* top | right | bottom | left */    
  margin: 2px 1em 0 auto;
}
```

You can't just make up your own variadic attributes - not all properties accept variadic attributes, and those which do expect values to be set in a particular order \(eg, if three values are set for `margin`, they will be interpreted as the values for the `top`, `horizontal` \(ie, `left` and `right`\) and `bottom` margins\).

### Floats and clears <a id="floats-and-clears"></a>

I'm not going to go into this that much, but these three articles will help explain floats \(and when they should/n't be used\). For the most part, avoid floats and clears; stick to `display: inline-block` or `display: inline` instead. Floats were designed to allow text to be wrapped around an image, and should only really be used in that context.

* ​[CSS Tricks - All About Floats](https://css-tricks.com/all-about-floats/)​
* ​[Smashing Magazine's The Mystery of Floats](http://www.smashingmagazine.com/2009/10/19/the-mystery-of-css-float-property/)​
* ​[Farewell Floats: The Future of CSS Layout](http://designshack.net/articles/css/farewell-floats-the-future-of-css-layout/)​

## Homework <a id="homework"></a>

_"Hell is other people's HTML"_

* Complete: HTML Labs​
  * Brook & Lyn
  * Busy Hands
  * eCardly
* Play: [CSS Diner](http://flukeout.github.io/)​
* Read: [Learn Layout](http://learnlayout.com/)​
* Memorize: [30 CSS Selectors](http://code.tutsplus.com/tutorials/the-30-css-selectors-you-must-memorize--net-16048)​
* Look at [this great reference comparing css properties and selectors by MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Values_and_units)​
* Explore the [chrome dev tools in more depth](https://developers.google.com/web/tools/chrome-devtools/) 



