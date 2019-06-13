---
description: More jQuery and AJAX
---

# Day 02

What we covered today:

* [Google Books Solution with jQuery](https://github.com/wofockham/sei-31/tree/master/10-ajax/book-search-jquery)
* JavaScript - AJAX with jQuery
* [Flickr Search](https://github.com/wofockham/sei-31/tree/master/10-ajax/flickr-search)

Warmup

* [Allergies](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week07/day02_allergies)

## JavaScript - AJAX with jQuery <a id="javascript-ajax-with-jquery"></a>

jQuery gives us a bunch of methods that make AJAX a lot easier.

### How to do it <a id="how-to-do-it"></a>

There are so many ways of using AJAX with jQuery. The most flexible is using the `$.ajax` method, which has been around since jQuery 1.0. All of the other AJAX methods that you can use with jQuery use this behind the scenes.

```javascript
// Basic syntax:
$.ajax( url, { OPTIONS } );
// Example:
$.ajax( "/stats", { method: "GET" } );
​
// Preferred syntax:
$.ajax({ OPTIONS });
// Preferred syntax example:
$.ajax({
  url: '/post',
  type: 'GET',
  // etc
})
```

Here is a more complex example using the preferred syntax.

```javascript
// Using the core $.ajax() method
$.ajax({
​
    // The URL for the request
    url: "/post",
​
    // The data to send (will be converted to a query string)
    data: {
        id: 123
    },
​
    // The type of request
    type: "GET",
​
    // The type of data we expect back
    dataType : "json",
​
    // Code to run if the request succeeds.
    // The responseText is passed to the 'success' function as the 'data' argument
    success: function( data ) {
        $( "<h1>" ).text( data.title ).appendTo( "body" );
        $( "<div class=\"content\">").html( data.html ).appendTo( "body" );
    },
​
    // Code to run if the request fails.
    // The request, the status code of the request and the error thrown are passed to the 'error' function as arguments
    error: function( xhr, status, errorThrown ) {
        alert( "Sorry, there was a problem!" );
        console.log( "Error: " + errorThrown );
        console.log( "Status: " + status );
        console.dir( xhr );
    },
​
    // Code to run when the request is completed, regardless of success or failure
    complete: function( xhr, status ) {
        console.log( "The request is complete." );
    }
​
});
```

We could also use promises for this, instead of encapsulating the handlers \(success, error etc.\). We can chain jQuery AJAX methods together, and jQuery 'promises' that these will get run.

```javascript
$.ajax({...}).always(function () {
    // This will always run
}).done( function (data) {
    // This will run on success.
}).fail( function (data) {
    // This will run if there is an error
});
```

### jQuery AJAX convenience methods <a id="jquery-ajax-convenience-methods"></a>

jQuery also offers a number of 'convenience methods' that use `$.ajax` behind the scenes. These are shorthand for more complicated `$.ajax` functions, meaning they're simpler to write, but less customizable.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Convenience Method</th>
      <th style="text-align:left">Long form $.ajax function</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&#x200B;<a href="http://api.jquery.com/jQuery.get/">$.get</a>&#x200B;</td>
      <td
      style="text-align:left">
        <p><code>$.ajax({</code> 
        </p>
        <p><code>  dataType: dataType,</code> 
        </p>
        <p><code>  url: url,</code> 
        </p>
        <p><code>  data: data,</code> 
        </p>
        <p><code>  success: success</code> 
        </p>
        <p><code>});</code>
        </p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x200B;<a href="http://api.jquery.com/jQuery.getJSON/">$.getJSON</a>&#x200B;</td>
      <td
      style="text-align:left">
        <p><code>$.ajax({</code> 
        </p>
        <p><code>  dataType: &quot;json&quot;,</code> 
        </p>
        <p><code>  url: url,</code> 
        </p>
        <p><code>  data: data,</code> 
        </p>
        <p><code>  success: success</code> 
        </p>
        <p><code>});</code>
        </p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x200B;<a href="http://api.jquery.com/jQuery.getScript/">$.getScript</a>&#x200B;</td>
      <td
      style="text-align:left">
        <p><code>$.ajax({</code> 
        </p>
        <p><code>  dataType: &quot;script&quot;,</code> 
        </p>
        <p><code>  url: url,</code> 
        </p>
        <p><code>  success: success</code> 
        </p>
        <p><code>});</code>
        </p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x200B;<a href="http://api.jquery.com/jQuery.post/">$.post</a>&#x200B;</td>
      <td
      style="text-align:left">
        <p><code>$.ajax({</code> 
        </p>
        <p><code>  type: &quot;POST&quot;</code> 
        </p>
        <p><code>  dataType: dataType,</code> 
        </p>
        <p><code>  url: url,</code> 
        </p>
        <p><code>  data: data,</code> 
        </p>
        <p><code>  success: success</code> 
        </p>
        <p><code>});</code>
        </p>
        </td>
    </tr>
  </tbody>
</table>### jQuery `.load` <a id="jquery-load"></a>

### _JavaScript - AJAX with jQuery - Further Reading_ <a id="javascript-ajax-with-jquery-further-reading"></a>

* ​[jQuery Fundamentals - AJAX & Deferreds](http://jqfundamentals.com/chapter/ajax-deferreds)​
* ​[Learn jQuery - jQuery AJAX methods](https://learn.jquery.com/ajax/jquery-ajax-methods/)​
* ​[jQuery API Documentation - $.ajax](http://api.jquery.com/jquery.ajax/)​

## JS APIs

### _JavaScript - APIs - Useful Links_ <a id="javascript-apis-useful-links"></a>

* ​[GitHub - Todd Motto - Public APIs](https://github.com/toddmotto/public-apis)​

