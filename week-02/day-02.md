---
description: >-
  What does a church and software have in common? First we build them, then we
  pray...
---

# Day 02

What we covered today:

* CSS - visual formatting model
  * The box model
  * Display
  * Position
* CSS - Fonts
  * Google Fonts
  * Font Awesome
  * Custom Fonts

### Warmup

* [Scrabble](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week02/day02_scrabble)

### Classwork

* [Display](https://github.com/wofockham/sei-31/tree/master/02-html-css/display)
* [Position](https://github.com/wofockham/sei-31/tree/master/02-html-css/position)

## CSS - visual formatting model <a id="css-visual-formatting-model"></a>

The CSS visual formatting model is the set of rules used to process and render documents to visual media. Each element of a document is converted to one or more boxes, and the layout of each box is determined by its:

* Box model \(width, height, margins, padding, borders\)
* Display type \(ie, `display`; eg, block, inline, inline-block, etc\)
* The positioning scheme \(ie, `position`; eg, static, relative, absolute, etc\)
* Relationship to other elements in the document object model.

### The box model <a id="the-box-model"></a>

![](https://i.imgur.com/DSi2s3A.png)

Every HTML element generates a rectangular box. The 'box model' defines these boxes, their properties and how they are laid out according to the 'visual formatting model'.

The core box-model CSS properties are: size \(`height` `width`\), `margin`, `padding`, `border` and `box-sizing`.

#### _Size_ <a id="size"></a>

The size of the element's content box - the size and width of the element itself. The following properties can be used to control the size of the box:

* `height` and `width`
* `max-height` and `max-width`
* `min-height` and `min-width`

#### _Padding_ <a id="padding"></a>

Specifies the space between the content box and the box's border. The following properties can be used to control the box's padding:

* `padding` - a 'variadic' property that can be used to set some or all padding attributes of an element \(all sides, all sides in pairs, all sides individually\)
* `padding-top`, `padding-right`, `padding-bottom`, `padding-left`

#### _Margins_ <a id="margins"></a>

Specifies the space between the element and adjacent elements. The following properties can be used to control the box's margins:

* `margin` - a 'variadic' property that can be used to set some or all padding attributes \(all sides, all sides in pairs, all sides individually\)
* `margin-top`, `margin-right`, `margin-bottom`, `margin-left`

#### _Border_ <a id="border"></a>

Specifies the style of the box's border. There are a _lot_ of border properties \(eg `border-bottom-righ-radius`, etc\), but the following properties can be used to control the box's border:

* `border` - a shorthand property for setting the border's width, style and color properties
* `border-style`
* `border-width`
* `border-color`

### Display <a id="display"></a>

* [​Class demo​](https://github.com/wofockham/sei-31/tree/master/02-html-css/display)

`display` one of the most important properties of CSS to control the layout. The default display value for each element is dependant on what type of element it is. The default is `inline`.

#### _display: block_ <a id="display-block"></a>

* `display: inline;` - The default value
* `display: block;` - Takes up the full width of the parent element
* `display: inline-block;` - displays the elements next to each other but will use the full length of the page. The default will align the elements to the bottom. This can be changed with `vertical-align: top;`
* `display: none;` - will hide the element so it's not visible on the page
* `display: initial;` - converts the display back to what it was when the page was initially loaded.

### Position <a id="position"></a>

* ​[Class demo​​](https://github.com/wofockham/sei-31/tree/master/02-html-css/position)
* `position: static;` - static is the default. It lets the browser figure out where it wants to put the element.
* `position: absolute;` - You take control. You must specify the exact position in px where you want the element to be positioned. This element will be positioned relative to the `document` and will be removed from the document flow.
* `position: relative;` - You will give the parent element `position: relative;` then all the subsequent child elements will be positioned relatively to the parent element.
* `position: fixed;` - Positioned relative to the `window`. This will stay on screen even if you scroll through the page.
* `z-index` - You can use the z-index to change the stacking hierarchy. A higher z-index will bring the element to the top of the stack.

#### _Placeholder Images_ <a id="placeholder-images"></a>

* ​[Fill Murray](http://www.fillmurray.com/)​
* ​[Placeholdit](http://placehold.it/)​
* ​[Lorem pixel](http://lorempixel.com/)​
* ​[Place Bear](http://placebear.com/)​
* ​[Dummy Image](http://dummyimage.com/)​
* ​[Place Cage](http://www.placecage.com/)​

### _CSS - The Visual Formatting Model - Recommended Readings_ <a id="css-the-visual-formatting-model-recommended-readings"></a>

* ​[MDN - CSS Reference - Position](https://developer.mozilla.org/en-US/docs/Web/CSS/position)​
* ​[MDN - CSS Reference - Display](https://developer.mozilla.org/en-US/docs/Web/CSS/display)​
* ​[MDN - CSS Reference - Box-Sizing](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)​
* ​[MDN - CSS Reference - Box-Model](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Box_model)​
* ​[Learn CSS Layout](http://learnlayout.com/) - this is a really handy guide for getting your head around display and positioning, etc.
* ​[Jon Duckett - HTML and CSS](http://www.htmlandcssbook.com/)​

## CSS - Fonts <a id="css-fonts"></a>

### Google Fonts <a id="google-fonts"></a>

* Go to [Google Fonts](https://www.google.com/fonts) and add the fonts that you want to your Collection
* Once you have selected all your fonts, click Use \(bottom right\)
* Choose the styles that you would like, and the character set
* Choose the "@import" option and copy and paste the code into the top of your CSS file
* Reference the font with the code provided

### Using the fonts <a id="using-the-fonts"></a>

```css
@import url('https://fonts.googleapis.com/css?family=Rubik');
​
​body{  
  font-family: 'Rubik', sans-serif;
}
```

### Font Awesome <a id="font-awesome"></a>

* Go [here](http://fortawesome.github.io/Font-Awesome/get-started/) and either:
  * download the necessary files;
  * get the CDN link emailed to you; or
  * use this CDN link: `https://use.fontawesome.com/releases/v5.3.1/css/all.css` \(current as at September 2018\)
* Put a `<link>` to that the Font Awesome CSS in your document's `<head>`
* Go through [the list of Font Awesome icons](http://fortawesome.github.io/Font-Awesome/icons/) and click on the one you want
* Copy and paste the snippet into your HTML.

### Custom Fonts <a id="custom-fonts"></a>

To reference custom fonts, you need to have the fonts saved in your project. Reference them in this way - make sure this is at the top of the CSS file! Reference this particular font by using the font-family name you referred to.

```css
@font-face {  
  font-family: 'GT Pressura';  
  src:  url('GTPresurra.eot');  
  src:  local('GT Pressura'),        
        url('GTPressura.eot#iefix'),        
        url('GTPressura.eot') format("truetype"),        
        url("GTPressura.otf") format("opentype"),        
        url("GTPressura.woff") format("woff"),        
        url("GTPressura.woff2") format("woff2"),        
        url("GTPressura.svg") format("svg");
}
```

To convert fonts, use [this tool.](http://onlinefontconverter.com/)​

### Additional resources <a id="additional-resources"></a>

* ​[Wireframing](https://wireframe.cc/)​

### Homework <a id="homework"></a>

* ​[Famous Scientist's Website​](https://gist.github.com/wofockham/47097f750914f9f23644)

