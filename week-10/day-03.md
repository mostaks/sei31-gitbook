---
description: '"Pfft-frack-Pop-sproing-Bing" - Don Martin'
---

# Day 03

#### What we covered today:

* [Express Generator - Intro](https://github.com/wofockham/sei-31/tree/master/14-node/express-generator-intro)
* [Node Scraping](https://github.com/wofockham/sei-31/tree/master/14-node/don_martin)
* [Paintr](https://github.com/wofockham/sei-31/tree/master/13-advanced/paintr)
* [Stupid Front End Tricks](https://github.com/wofockham/sei-31/tree/master/13-advanced/stupid_frontend_tricks)
* [Interview Q's](https://gist.github.com/wofockham/3a1d8229a7a09e4c0f98b2d3989311b4)
* YouTeach
  * Ben: Professional Gambling

#### Warmup

* [Collatz Conjecture](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week10/day03_collatz_conjecture)

## Express

\(View Yesterday's page for Express notes\)

## Stupid Front-End Tricks

```text
mkdir stupid
cd stupid/
touch index.html
mkdir css
mkdir js
touch css/stupid.css
touch js/stupid.js
curl http://code.jquery.com/jquery-1.12.4.js > js/jquery.js
atom .
```

Open the index.html file and add the below code. Remember you can use emmet on the `p*10>lorem`line to populate the paragraph tags with lorem ipsum. We will link the two script tags and the css file.

```text
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <link rel="stylesheet" href="css/stupid.css">
  <script src="js/jquery.js" charset="utf-8"></script>
  <script src="js/stupid.js" charset="utf-8"></script>
  <title>Stupid Front-End Tricks</title>
</head>
<body>

  <div class="container">
    <h1>Stupid Fron End tricks</h1>
    p*10>lorem

    <div class="bill">

    </div>
    p*10>lorem

  </div>

</body>
</html>
```

Add some styling to the CSS file.

```text
body {
  background-image: url(http://placekitten.com/1024/1025);
  background-size: cover;
}

.container {
  max-width: 960px;
  margin: 0 auto;
  background-color: rgba(255, 255, 255, 0.6);
  padding: 2em;
  box-sizing: border-box;
  font-size: 140%;
}

.bill {
  height: 10em;
  background-image: url(http://fillmurray.com/900/1200);
  background-size: cover;
}
```

Now we want to start doing something with the image of Bill Murray. Select the `div` with the class of `.bill` and save it in a variable. Remember the naming convention when selecting an element with `jquery` we start it with a `$`. Then we also need to place an event listener on the `window` that will be waiting for a scroll event.

```text
$(document).ready(function(){
  // Parallax effects
  const $bill = $('.bill');

  $(window).on('scroll', function(){
    console.log(`scroll`);
  });
});
```

Run the code, open the console and check to see that we're getting a console log with the string of `scroll`.

We now want to get the scrollTop and save that in a variable.

```text
$(document).ready(function(){
  // Parallax effects
  const $bill = $('.bill');

  $(window).on('scroll', function(){
    const scrollTop = $(window).scrollTop();
    console.log(scrollTop);
  });
});
```

Open the console and scroll the page to make sure you have the correct values outputting.

Now we want to make the image of Bill Murray move at a different pace to the body when we scroll.

```text
$(window).on('scroll', function(){
  const scrollTop = $(window).scrollTop();
  $bill.css('background-position-y', -scrollTop / 3);
});
```

We can use the same technique to change the scroll speed of the `background-image`.

```text
$(document).ready(function(){
  // Parallax effects
  const $body = $('body');
  const $bill = $('.bill');

  $(window).on('scroll', function(){
    const scrollTop = $(window).scrollTop();
    $bill.css('background-position-y', -scrollTop / 3);
    $body.css('background-position-y', -scrollTop * 0.75);
  });
});
```

This is all well and good but what this page now needs is BUBBLES! Create another event listener that will watch out for when the mouse has been moved of the page.

```text
$(document).ready(function(){
  // Parallax effects
  const $body = $('body');
  const $bill = $('.bill');

  $(window).on('scroll', function(){
    const scrollTop = $(window).scrollTop();
    $bill.css('background-position-y', -scrollTop / 3);
    $body.css('background-position-y', -scrollTop * 0.75);
  });

  $(window).on('mousemove', function(e){
    console.log( e );
  });
});
```

Open the console and have a look at the object that is getting logged out. If you open the object you will see all the event information that is being recorded. ALl was care about is the `pageX` and `pageY`values.

Lets use those values to place some bubbles on the page. Define what a bubble is and append it to the body element.

```text
$(window).on('mousemove', function(e){
  // const x = e.pageX;
  // const y = e.pageY;
  // same as below
  const {pageX: x, pageY: y} = e; // Destructuring

  const $bubble = $('<div class="bubble"></div>').css({
    left: x,
    top: y
  }).appendTo($body);
});
```

This will create the bubbles but they have no css so we cant see them. We need to add some more CSS to the `style.css` file.

```text
.bubble {
  position: absolute;
  width: 10em;
  height: 10em;
  border: 1px solid aliceblue;
  border-radius: 100%;
}
```

Now when you mouse around they will appear on the page.

Bubbles are as unique as a snowflake so we don't want them to have the same size each time they are populated on the page. Lets add some randomisation.

```text
$(window).on('mousemove', function(e){
  // const x = e.pageX;
  // const y = e.pageY;
  // same as below
  const {pageX: x, pageY: y} = e; // Destructuring

  const size = (Math.random() * 10) + 'em';

  const $bubble = $('<div class="bubble"></div>').css({
    left: x,
    top: y,
    width: size,
    height: size
  }).appendTo($body);
});
```

Looking pretty cool! But don't bubbles float?

Lets get our bubble to move to the top of the page.

```text
$(window).on('mousemove', function(e){
  // const x = e.pageX;
  // const y = e.pageY;
  // same as below
  const {pageX: x, pageY: y} = e; // Destructuring

  const size = (Math.random() * 10) + 'em';

  const $bubble = $('<div class="bubble"></div>').css({
    left: x,
    top: y,
    width: size,
    height: size
  }).appendTo($body);

  $bubble.animate({ top: -200 }, 2000);
});
```

They're now looking and acting like real bubbles but if you open the `elements` tab in the console you'll see that they're just bunching up at the top and using all our memory. Lets fix that!

There are a couple of ways that we could approach this: We could add a set time out and remove the bubbles from the page once the timer reaches the end.

```text
$(window).on('mousemove', function(e){
  // const x = e.pageX;
  // const y = e.pageY;
  // same as below
  const {pageX: x, pageY: y} = e; // Destructuring

  const size = (Math.random() * 10) + 'em';

  const $bubble = $('<div class="bubble"></div>').css({
    left: x,
    top: y,
    width: size,
    height: size
  }).appendTo($body);

  $bubble.animate({ top: -200 }, 2000);

  setTimeout(function(){
    $bubble.remove();
  }, 2000);
});
```

But this means you would have to change two timers. First in the `setTimeOut` but also in the `animate`function.

A better method would be to add another function to the end of your animation, as a second argument, and remove the bubble.

```text
$bubble.animate({ top: -200 }, 2000, function(){
  $bubble.remove(); //Clean up after ourselves to conserve memory.
});
```

## Event Delegation and Paintr

* [Class Demo](https://github.com/wofockham/sei-31/tree/master/13-advanced/paintr)

```bash
rails new paintr -T --skip-git
cd paintr/
rails g controller Pages index
atom .
```

We need to require the `jquery` gem in the `Gemfile`. While you're at it remove `gem turbolinks`

```ruby
gem 'jquery-rails'
```

We now need to bundle to make sure the gem is working.

```bash
bundle
```

We have `jquery` required in the `Gemfile` but we also need to do the same in the `application.js` file in this directory `app/assets/javascript/application.js`. Remove `turbolinks` and replace it with `jquery`.

```javascript
//= require rails-ujs
//= require jquery
//= require_tree .
```

Now we need to set up our routes in the `routes.rb` file.

```ruby
Rails.application.routes.draw do
  root :to => 'pages#index'
end
```

Now start the server from the terminal.

```bash
rails server
```

Change the `index.html.erb` to `index.haml` and add the below code.

```ruby
%h1 Paintr

= text_field_tag :color, '#FFF', :autofocus => true
- button_tag 'Add', :id => 'add-color'

%div.palette

%div.canvas
  - 10_000.times do
    %div.pixel
```

We will now add some css in the `paintr.scss` in the `stylesheets` folder.

```css
.pixel {
  float: left;
  width: 1em;
  height: 1em;
  border: 1px solid black;
  display: inline-block;
}
```

Create a new file in the `javascripts` directory called `paintr.js`.

```javascript
$(document).ready(function(){
  $('#add-color').on('click', function(){
    const color = $('#color').val();
    const $swatch = $('<div />').addClass('swatch').css('background-color', color);
    $swatch.appendTo('.palette');
  });
});
```

Now we want to set up the css for the swatch.

```css
.swatch {
  width: 5em;
  height: 5em;
  border: 1px solid black;
  display: inline-block;
  margin-top: 1em;
  margin-right: 1em;
  border-radius: 0.5em;
}
```

At the moment our input is a little boring, rather than having our user put the hex value in lets make it a little easier. We have access to a rails helper called `color_field_tag`. Let try it out. Change the input to a `color_field_tag`.

= color\_field\_tag :color =&gt; '\#FFF', :autofocus =&gt; true, :type =&gt; 'color'

Now the user can pick from the color wheel.

Back in the `paintr.js` file lets create event listener that watches for which swatch color is selected.

```javascript
$('.swatch').on('click', function(){
  $(this).addClass('selected');
  console.log(`new color selected`);
});
```

Unfortunately this doesn't work. The reason is because the .swatch doesn't exist when the event listener is attached.

To get around this we can delegate the handler to the parent.

```javascript
$('.palette').on('click', '.swatch', function(){
  $(this).addClass('selected');
  console.log(`new color selected`);
});
```

To prevent having multiple swatches selected we have to remove the class from the original swatch.

```javascript
$('.palette').on('click', '.swatch', function(){
  $('.selected').removeClass('selected');
  $(this).addClass('selected');
  console.log(`new color selected`);
});
```

Get the pixels to change color so we can paint with them.

```javascript
$('.pixel').on('mouseover', function(){
  const color = $('.swatch.selected').css('background-color');
  console.log(color);
});
```

Reload the page, put a color in the swatch and open the console. Select the color then mouse over the pixels. There should be a log in the console stating the color.

Now we need to use that color.

```javascript
$('.pixel').on('mouseover', function(){
  const color = $('.swatch.selected').css('background-color');
  $(this).css('background-color', color);
});
```

This is an extremely expensive way of attaching these listeners. It will create ten thousand copies and attached it to each pixel. We can use event delegation again to only attached one copy to the parent.

```javascript
$('.canvas').on('mouseover', '.pixel', function(){
  const color = $('.swatch.selected').css('background-color');
  $(this).css('background-color', color);
});
```

**Bonus**

Now we want to be able to use a button to stop drawing. We can do this by passing the event as an argument to the function.

```javascript
$('.canvas').on('mouseover', '.pixel', function(event){

  console.log(event);

  const color = $('.swatch.selected').css('background-color');
  $(this).css('background-color', color);
});
```

If you now mouse over the pixel you will see the event object being returned. You will see within there that it is keeping track of the shift key `shiftKey => false`. We can use this to stop the pixels from changing color.

```javascript
$('.canvas').on('mouseover', '.pixel', function(event){

  if(event.shiftKey){
    return;
  }

  const color = $('.swatch.selected').css('background-color');
  $(this).css('background-color', color);
});
```

**Want more?**

You should now try and add a background image behind the pixel so you can draw over it.



