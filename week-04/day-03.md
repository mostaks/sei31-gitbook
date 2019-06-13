---
description: The best revenge is massive success - Frank Sinatra
---

# Day 03

What we covered today:

* Web Servers
* Sinatra
* [Guest Speaker's slides - Taryn: CSS](https://docs.google.com/presentation/d/1OK_ZfmdIVS-cKkP4cqjCbK7-6vPGtnPgtLY5bbeWfvU/edit?usp=sharing)

#### Warmup

* ​[Scrabble - Ruby](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week04/day03_ruby_scrabble)​​

#### Classwork

* ​[Calculator](https://github.com/wofockham/wdi-30/tree/master/06-sinatra/calculator)​​
* ​Stock [Quote](https://github.com/wofockham/wdi-30/tree/master/06-sinatra/wallstreet)​

## Web servers <a id="web-servers"></a>

**TL;DR**: A web server is a program that uses HTTP to serve files in response to requests sent to a URL from a user's HTTP client.

**Wait, what?**

* **HTTP**: HyperText Transfer Protocol - the network protocol used to distribute information on the web.
* **HTTP client**: The browser, which is used to send a request to a web server and receive the web server's response. AKA the "user agent".
* **URL**: Uniform Resource Locator - a reference to a resource that specifies: 1. The location of that resource on a computer network; and 2. The mechanism for retrieving it.

  A URL has a number of components - this is an example of a typical URL:

​[http://www.lol.badgerville.com/users/luke/images/trails.html?title=green\#hectic](http://www.lol.badgerville.com/users/luke/images/trails.html?title=green#hectic)​

| Protocol | Domain | Path | Resource or Query | Fragment |
| :--- | :--- | :--- | :--- | :--- |
| http:// | www \(_subdomain_\) | users/luke/images | trails.html \(_resource_\) | hectic |
| - | lol \(_local hostname_\) | - | ?title=green \(_query_\) | ​ |
| - | badgerville \(_2nd level domain_\) | - | - | ​ |
| - | com \(_1st level domain_\) | - | - | ​ |

* **Protocol** \(more accurately ‘scheme’\): How to connect to the resource.
* **Domain** \(more accurately ‘host’\): Where to connect to the resource.
* Path + Resource/Query String: What to ask for.
  * **Path**: the path to the resource.
  * **Resource**: the resource the client is seeking.
  * **Query String**: logic for accessing the resource the client is seeking.
* **Fragment**: an option that is processed by the client, not the server.

> You don't need to worry about this right now, but - for the sake of completeness - for websites, the domain/host represents the literal IP address of the host of the resource. The client \(browser\) sends the URL to a Domain Name Server, which finds the IP address mapped to the domain name. The client then connects to that IP address and sends the HTTP request to the web server. The web server processes the request and responds to the client.\*

The web server receives HTTP requests from a browser, processes that request, and determines what content to serve to the browser in response to that request. While the primary function of a web server is - as the name would suggest - to _serve_ content in response to a request, a web server can also receive content from the client and route it for storage.

When it receives an HTTP request, the web server maps the path component of a URL to:

* A local file system resource \(for static requests\);
* An internal or external program \(for dynamic requests\).

### Starting a local web server using Python <a id="starting-a-local-web-server-using-python"></a>

You can use Python's 'SimpleHTTPServer' module to create a web server on your machine - you could, potentially, then host your application on the world wide web using your machine.

The following terminal command will create a web server using the contents of the current working directory:

```bash
# If Python version is 2.X
python -m SimpleHTTPServer
​
# If Python version is 3.X
python -m http.server
​
# => Serving HTTP on 0.0.0.0 port 8000 ...
```

Then navigate to **localhost:8000** in your browser.

## Sinatra <a id="sinatra"></a>

#### _What is it?_ <a id="what-is-it"></a>

Sinatra is a **Domain Specific Language** \(DSL\). A DSL is a language designed to solve problems in a particular 'domain' \(domain as in 'field of stuff'\). In this case, Sinatra is a is a DSL for creating web servers; it is a web application framework. Sinatra is a 'gem' \(a Ruby library\) that we include to give us methods we can use to construct simple web applications \(with minimal effort\).

#### _How do we use it?_ <a id="how-do-we-use-it"></a>

It is a gem like anything else!! We need two gems for this though - we have **sinatra** itself \(which gives us all of those necessary methods\), and we have **sinatra-contrib** - which is anything worthy that has been contributed to the Sinatra gem. This is what includes the live reloader etc.

First, we need to install the sintra and sintra-contrib gems. Run these commands in your terminal:

```bash
gem install sinatra
gem install sinatra-contrib
```

For a basic Sinatra application, we don't need that much. We just need to `require` the necessary gems in our Ruby file. We call the main Sinatra file **main.rb**.

```ruby
require 'sinatra'           # Gets sinatra itself
require 'sinatra/reloader'  # Live reloader from sinatra-contrib
```

This is all that is required to technically run the server \(but it won't do anything or respond to anything\).

To run the server, we just use Ruby to run the file from our terminal - `ruby main.rb`. Normally this will run a server at localhost:4567.

**NOTE**: If you run `ruby main.rb` but already have an open server, you'll run into a problem, since the default port \(4567\) is already in use.

### Routes <a id="routes"></a>

In Sinatra, a route is an HTTP method paired with a URL path pattern. Each route has a block.

```ruby
get '/' do  
  "This is a 'get' request to the root path ('/') "
end​
​
get '/anything' do  
  "This is a 'get' request to the '/anything' path " 
  # This is returned as a response
end
​
​get '/hello' do  
  "Hello World"
end
```

In the second example above:

* `get` is the HTTP method;
* `/anything` is the path - the pattern which the URL must match.

Routes are matched in the order they are defined. The first route that matches the request path is the route that will be invoked - all subsequent routes that match the method and pattern of the path in the request will be ignored.

In the third example above:

You can go to `http://localhost:4567/hello` once you have started the server and you will see the text `Hello World` on the screen.

#### _Literal paths_ <a id="literal-paths"></a>

Paths like `/anything` are literal paths. They require the request to be a literal match with a path specified in a route - we actually have to visit `/cats` in order for our web server to respond to the request - this is annoying because we don't always know what we are going to receive from the client, and requires us to create routes for _every single_ resource.

#### _Dynamic paths_ <a id="dynamic-paths"></a>

The way we solve this is by using named parameters rather than literal paths wherever possible. Whatever is matched by the thing prefixed with the colon is stored in the a hash called `params` \(which is automatically generated for us\).

```ruby
get '/hello/:name' do  
  # matches "/hello/the-blade" or "/hello/josh" or anything else that starts with 
  #"/hello/"  
  # params[:name] might be 'the-blade' or 'josh'​  
  
  "Hello #{ params[:name] }!"
end
​
​get '/multiply/:x/:y' do  
  result = params[:x].to_f * params[:y].to_f  
  "The result is #{ result }"
end  
​
# going to the path http://localhost:4567/multiply/2/3/ will display 
# => 'The result is 6'
```

### Views <a id="views"></a>

At this point, the blocks in our routes simply has a string response - we are just rendering text to the page. That isn't ideal. We want it to be well presented!! In its most basic form, we use ERB \(Embedded Ruby\) - Ruby code embedded in HTML.

Sinatra is very smart, but requires a very specific directory and file setup. For it to work, we need a **views** folder. This is where all of our HTML \(stored as ERB files\) will go.

The first thing we always do is create a **layout.erb** file. This will be rendered by default every time Sinatra sees the `erb :template_name` command in a route. This is where all of the stuff that you want to be on every page should go. Here's an example of a basic **layout.erb**.

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="/master.css">
    <title>Our Basic Sinatra App</title>
</head>
<body>
    <nav>
        <ul>
            <li><a href="/home">Home</a></li>
            <li><a href="/about">About</a></li>
            <li><a href="/gallery">Gallery</a></li>
            <li><a href="/contact">Contact</a></li>
        </ul>
    </nav>
​
</body>
</html>
```

This will get loaded regardless of the request. It's missing one very important thing - the `<%= yield %>` method, which we'll cover in a minute.

If you imagine a blog situation, you might want a navigation that is on every page - this goes on the layout.erb page. But you might also want to show the actual post as well. This is what ERB is great at! You pass in a symbol \(the name of the file you'd like to load in\) to the ERB method and it goes and grabs it for you.

```ruby
get '/post' do    
  erb :post
end
```

If you just did this though, it would get the file but wouldn't know where to put the contents of that particular file in the layout! As such, it will only show the layout.erb file. We use the `yield` method to solve this problem! This grabs the contents of the file requested and inserts it into the layout.

```ruby
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
​
    <%= yield %>
​
</body>
</html>
```

### Creating forms <a id="creating-forms"></a>

Forms are used to retrieve information from the user.

```markup
<form action="/calc">
  First number: <input type="number" name="x" placeholder="7">
  Operator: <input type="text" name="operator" placeholder="+">
  Second number: <input type="number" name="y" placeholder="12">
​
  <button>Calculate!</button>
</form>
```

When the user fills in the inputs it will be submitted to the server under the action of '/calc'. It will then generate a URL with the query attached.

`http://localhost:4567/calc?x=7&operator=%2B&y=12/`

From there you will need to set up the `get` action in your main.rb file.

```ruby
get '/calc' do    
  # insert calculations and case statement here  
end
```

### Static assets <a id="static-assets"></a>

In a basic Sinatra application, we would store static assets like stylesheets, images, etc, in a folder called **public**, but in this case, we don't need to specify the public folder in our path - if we have a stylesheet saved at public/style.css, the link in our layout.html to that file would simply be `<link rel="stylesheet" href="/style.css">`.

### Basic Sinatra file structure <a id="basic-sinatra-file-structure"></a>

So, the file structure of a basic Sinatra web app should be something like this:

```bash
|- my_application
|    |- main.rb
|    |- views
|         |- layout.erb
|         |- home.erb
|         |- contact.erb
|         |- about.erb
|    |- public
|         |- style.css
|         |- images
|              |- logo.png
```

NOTE: This structure is important - Sinatra expects your view templates to be in a folder called 'views', and it expects public files to be in a folder called 'public'. The filename 'layout' is also important - if Sinatra sees a file in your views folder called **layout.erb**, it will automatically use this as the master template wrapping all your other templates.

### ERB syntax <a id="erb-syntax"></a>

Embedded Ruby \(aka 'eRuby' or 'erb'\) is a templating system. It allows us to embed Ruby code in our HTML\* to handle dynamic content \(ie, content that we can't just hard code in our HTML, but which will vary depending on the request received by the web server\).

* _We can actually use ERB in conjunction with any plain text document type, but for the most part we're only interested in using ERB for HTML templating._

We use erb files for our views/templates.

Any Ruby code can be written in an erb file using erb tags, but it's best to keep as much logic out of our views as possible. So, in the basic Sinatra implementation set out in this guide, we want to put most of our logic in our **main.rb** file.

There are a few different erb tags, but these are the three we'll use the most:

```markup
<%  ruby_code %>          <!-- execute ruby code without displaying the result  -->
<%= ruby_expression  %>   <!-- replace with the result of the expression  -->
<%# comment %>            <!-- ignored, and not sent to client -->
```

### Accessing Ruby variables in view templates <a id="accessing-ruby-variables-in-view-templates"></a>

We'll often create and manipulate data in our main.rb file - for example, taking data from a form in one view, manipulating that data, and passing that data to another view. In order to make Ruby variables declared in a **main.rb** method accessible in the method's corresponding view, we need to create an instance variable. Creating an instance variable is simple - just prefix your variable name with a `@` symbol \(note - there is no direct relationship between `@variable` and `variable` - these can represent completely different objects\).

Say that we want to make a collection of animals available to our **animals** view. First, in our **main.rb**, we would go to the route for that view, and create the array:

```ruby
get '/animals' do  
  animals = ["badger", "wolf", "goat", "chicken"]  
  erb :animals
end
```

If we attempt to access the `animals` variable in our **animals.erb** template \(using ERB tags like so: `<% animals %>`\), we would get an error saying that the variable `animals` is not defined.

In order to make this array available in our view, we need to create an instance variable.

```ruby
get '/animals' do​  
  @animals = ["badger", "wolf", "goat", "chicken"]  
  erb :animals
end
```

In our view, we can then access the instance variable `@animals` like so:

```markup
<div>  
  <% @animals.each do |animal| %>    
  <p> <%= animal %></p>    
  <% end %>
</div>
```

### _Embedded Ruby - Further Reading_ <a id="embedded-ruby-further-reading"></a>

* ​[API Dock - Embedded Ruby](http://apidock.com/ruby/ERB)​

### _Sinatra - Further Reading_ <a id="sinatra-further-reading"></a>

* ​[Sinatra Introduction](http://www.sinatrarb.com/intro.html)​

## Homework <a id="homework"></a>

* [​Books and Pair Programming Bot](https://gist.github.com/wofockham/1f3821464656bf2fb253)

