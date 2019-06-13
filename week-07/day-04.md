---
description: AJAX on Rails + Elm-lang
---

# Day 04

What we covered today:

* Ajax on Rails
* Elm
* [Presentation: Daisy Smith](https://docs.google.com/presentation/d/10useVTXp9dbreHH0vQpMgDPHezd9itlmshqroYlyw5s)

Warmup

* [Flatten and Reverse](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week07/day04_flatten_and_reverse)

## JavaScript - AJAX on Rails <a id="javascript-ajax-on-rails"></a>

Quick recap:

* Our rails server responds to HTTP requests to particular URLs.
* The router maps URL paths to controller actions, routing HTTP requests to the appropriate action based on the URL requested by the client.
* The controller action is executed, and - where necessary - a response is rendered to the client.

Up to this point, the controller has been rendering HTML in response to all HTTP requests, but a concept at the core of single-page applications is to respond to HTTP requests with JSON objects \(rather than HTML\), and update only those parts of the page that require updating, rather than re-rendering the entire page.

Rails works really well with AJAX and JSON objects. We can:

1. Render JSON in response to `GET` requests set via AJAX, by processing the request in the controller alone.
2. Retrieve data from the database and render JSON in response to `GET` requests sent via AJAX.
3. Destroy records in the database in response to `DELETE` requests sent via AJAX.
4. Update records in the database in response to `POST`/`PUT` sent via AJAX.

### Render JSON in response to `GET` requests set via AJAX, by processing the request in the controller alone <a id="render-json-in-response-to-get-requests-set-via-ajax-by-processing-the-request-in-the-controller-alone"></a>

This example illustrates how we can process a request made to a particular URL and send a response from the controller, without fetching any data from the database.

Our application will generate random cat names, because of course it will.

#### _Create a new Rails app_ <a id="create-a-new-rails-app"></a>

In terminal:

```bash
  rails new ajax_rails --skip-git --skip-turbolinks -d postgresql
  rails generate controller Pages index
```

#### _Set up our routes_ <a id="set-up-our-routes"></a>

In **config/routes.rb:**

```ruby
# This will show app's single 
page:root 'pages#index'
# Our AJAX requests will be sent to this path:
get "/generator" => "pages#generator"
```

#### _Set up our app's single HTML view_ <a id="set-up-our-apps-single-html-view"></a>

In **app/views/pages/index.html/erb**:

```markup
<button id="cat-name">Cat Name Generator</button>
​
<p>Introducing
  <span id="title"></span>
  <span id="name"></span>
  <span id="surname"></span>
</p>
```

#### _Set up our controller actions_ <a id="set-up-our-controller-actions"></a>

Our application is going to have two actions: an `index` action, and a `generator` action. Only the `index` action will render HTML; the `generator` action will render JSON.

By default, the controller will look for an HTML template app/views/pages\|layouts/generator.html in response to requests to the `/generator` path. But we want the action to send JSON to the browser. Here's the line we'll add to the end of our `generator` action in **app/controllers/pages\_controller.rb** to get this to work:

```ruby
def generator  
  render :json => @generated_name
end
```

Two things are happening here: firstly, our action will render `@generated_name` as JSON instead of HTML. Secondly, the `:json` method will call `.to_json` on the `@generated_name` object, converting it to a JSON object that we can access and manipulate in using JavaScript before it is rendered by the browser.

Now we're going to add the logic the server needs to process the request to our controller. We'll create a hash of cat name options, randomly select values from that hash and store them in an instance variable called `@generated_name` which we'll send to the browser as a JSON object.

```ruby
class PagesController < ApplicationController
  def index
  end
​
  def generator
    options = {
      male: {
        titles: ["Mr ", "Lord ", "Count ", "Sir ", "His Lordship, "],
        names: ["Lewis ", "Felix ", "Baxter "]
      },
      female: {
        titles: ["Ms ", "Lady ", "Countess ", "Baroness ", "Her Ladyship, "],
        names: ["Kristy ", "Liesl ", "Pia "]
      },
      surnames: ["Bergstrom", "Eckersley", "Weinberg"]
    }
​
    random = ["male", "female"].sample.to_sym
​
    @generated_name = {
      :title => options[random][:titles].sample,
      :name => options[random][:names].sample,
      :surname => options[:surnames].sample
    }
​
    render :json=> @generated_name
  end
end
```

If we wanted our application to be able to render both HTML and JSON in response to requests to the `/generator` URL, we could use Rails' `respond_to` method to respond to different request formats. Why would we do this? Just say we wanted to be able to render HTML in our own application's UI, but also wanted to open up our application as an API to other applications. We'd first have to create a new **app/views/pages/generator.html.erb** view to render our HTML. Then, our `render` line could be changed as follows to allow this:

```ruby
respond_to do |format|
  # This will render generator.html in response to requests for HTML
  format.html 
  # This will render JSON to AJAX requests
  format.json { render json: @generated_name } 
end
```

#### _Set up our AJAX request and response handler_ <a id="set-up-our-ajax-request-and-response-handler"></a>

Create a new file **app/assets/javascripts/generator.js**.

We need to:

1. Add an event listener to the button.
2. Create a function that will send a `GET` request via AJAX to the path `/generator`.
3. Include a `.done` function that will handle the JSON response returned by the server and update the page.

```javascript
$(document).ready(function() {
​
  var randomName = function() {
    $.ajax({
      url: '/generator',
      type: "GET",
      dataType: "JSON"
    }).done(function(data) {
       $("#title").text(data.title);
       $("#name").text(data.name);
       $("#surname").text(data.surname);
    });
  };
​
  $("button").on("click", randomName);
​
});
```

Open your server \(`rails s`\) and you should be able to generate a cat name!

### Retrieve data from the database and render JSON in response to `GET` requests sent via AJAX <a id="retrieve-data-from-the-database-and-render-json-in-response-to-get-requests-sent-via-ajax"></a>

This example illustrates how we can retrieve data from our database and send it back to the browser in response to an AJAX request made to a particular URL, which is - arguably - more useful than just getting our controller to generate a random cat name.

In this example we won't be just sending back a record \(or series or records\), we'll be sending back:

* A record;
* That record's _associated_ records;
* What a particular method in that record's model returns.

Let's use the app created above as a starting point.

#### _Scaffold some models_ <a id="scaffold-some-models"></a>

In terminal:

```bash
  rails generate scaffold Habitat name:string
  rails generate scaffold Species name:string status:string habitat:references
  rails generate scaffold Animal name:string color:string species:references
```

#### _Update our models' associations_ <a id="update-our-models-associations"></a>

The scaffold will have set up the `belongs_to` associations for you \(both the migrations with the necessary foreign keys and the associations on our models\), but you'll need to add the `has_many` associations to your models.

We're also going to add another method to the Animal class that we can call on an animal object to get information about the animal's climate. We'll see below how we can include this method in our JSON response.

Update your models so they look like this:

```ruby
# in app/models/animal.rb
class Animal < ActiveRecord::Base
  belongs_to :species
​
  def climate
    if self.species.habitat.name == "Alpine"
      "Cold"
    else
      "Maybe not too cold"
    end
  end
end
​
end
​
# in app/models/species.rb
class Species < ActiveRecord::Base
  has_many :animals
  belongs_to :habitat
end
​
# in app/models/habitat.rb
class Habitat < ActiveRecord::Base
  has_many :species
end
```

Now we'll have the following models and associations:

* Animal `belongs_to :species`
* Species `belongs_to :habitat`, `has_many :animals`
* Habitat `has_many :species`

#### _Set up our database_ <a id="set-up-our-database"></a>

In **db/seeds.rb**:

```ruby
h1 = Habitat.create({:name => "Woodland"})
h2 = Habitat.create({:name => "Alpine"})
​
s1 = Species.create({:name => "Meles meles", :status => "Not endangered"})
s2 = Species.create({:name => "Martes americana", :status => "Endangered"})
s3 = Species.create({:name => "Mustela putorius", :status => "Endangered"})
​
a1 = Animal.create({:name => "Badger", :color => "Black and white"})
a2 = Animal.create({:name => "Pine marten", :color => "Brown and white"})
a3 = Animal.create({:name => "Polecat", :color => "White"})
​
s1.animals << a1
s2.animals << a2
s3.animals << a3
​
h1.species << s1
h2.species << s2 << s3
```

Then, in terminal:

```ruby
rails db:create
rails db:migrate
rails db:seed
```

#### _Set up a route_ <a id="set-up-a-route"></a>

We're going to put an additional route to handle AJAX requests to get more details about our animals. Any `GET` requests to the URL `/details/{something}` will be routed to a `details` action in the `animals_controller`.

Add the following to your **config/routes.rb** file

```ruby
  get 'details/:id' => "animals#details", :as => 'details'
```

#### _Configure the controller_ <a id="configure-the-controller"></a>

We need to create a `details` action in **app/controllers/animals\_controller.rb**. In there, we'll be retrieving the records we want from the database and sending certain details of those records \(and those records' associated records\) back to the browser as JSON objects.

```ruby
# ...
def details
​
  # Get an animal record from the database using the ID in params.
  @animal = Animal.find(params[:id])
​
  # If we just want to return details of the record represented by the @animal object, we could now simply do:
​
  # render :json => @animal
​
  # But if we want to get details of OTHER records associated with the @animal object, we need to call .to_json on the @animal object, and specify what associated records we want to include, and what attributes of those associated records we want to include/exclude.
  @response = @animal.to_json(
  :methods => :climate,
  :include => {
    :species => {
      :except =>
        [:created_at, :updated_at],
      :include => {
        :habitat => {
          :only => :name
        }
      }
    }
  })
​
  # Then we render the response as :json. This lets Rails know to render a JSON object back to the browser, rather than an HTML view.
​
  render :json => @response
  # ...
end
```

If you open up the URL **localhost:3000/details/1** in the browser, you should see a representation of a JSON object relating to the first record in our database's animals table.

#### _Set up a view_ <a id="set-up-a-view"></a>

First, delete all the content in the template **app/assets/views/animals/index.html.erb**, and replace it with the following:

```markup
<% @animals.each do |animal| %>
  <div id="#{animal.id}">
  <%= link_to(animal.name, details_path(animal.id), remote: true, :class => "get-animal") %>
  </div>
<% end %>
<div class="response-text"></div>
```

This is going to create a link to the `details_path`, which will be routed to the `details` action in our `animals controller`. Notice the `remote: true` - this will add the HTML attribute `data-remote=true` to the `a` element generated by the `link_to` helper, and tells the browser to send the request to the server using AJAX. We're also giving the `div` the class of the animal's ID to make it easier to remove the element from the DOM \(in response to a successful delete request, which we'll do a bit later\).

We've also created a `div` element \(with the class `response-text`\) where we'll be putting parts of the response text returned by the server.

#### _Set up the AJAX response handler_ <a id="set-up-the-ajax-response-handler"></a>

Now we'll use JavaScript to handle the response from the server and render it in the browser.

I'm going to rename the **app/assets/javascripts/animals.coffee** generated by the scaffolding to **app/assets/javascripts/animals.js** and put all my JavaScript in there.

To start with, I'd put a `debugger` in the jQuery AJAX response handler, so I can figure out how to access the data we need in the object that the request returns, but we'll skip over that step below.

We can pass up to four arguments into the AJAX response handlers - the `event` \(particularly useful if we want to manipulate the element that was clicked on\), the `data` \(the JSON object returned by the controller\), the `status` \(eg 'success', 'error'\) and the full `xhr` response. We'll only be using the `data` here.

```javascript
$(document).ready(function() {
  $(".get-animal").on("ajax:success", function(event, data, status, xhr) {
    // debugger
    $animal = $("<p>").text("Animal: " + data.name);
    $species = $("<p>").text("Species: " + data.species.name);
    $status = $("<p>").text("Habitat: " + data.species.habitat.name);
    $(".response-text").empty().append($animal).append($species).append($status);
  }).on("ajax:error", function() {
    $error = $("<p>").text("An error occurred. That's all we know.");
    $(".response-text").empty().append($error);
  });
});
```

Browse to **localhost:3000/animals** and you should be able to click on an animal's name and see the page update with data from the database without the browser reloading the whole page.

### Destroy records in the database in response to `DELETE` requests sent via AJAX <a id="destroy-records-in-the-database-in-response-to-delete-requests-sent-via-ajax"></a>

This is pretty straightforward. We need to:

1. Add another link to our view
2. Slightly modify how the `destroy` action in the controller will repond to a `DELETE` request
3. Add another AJAX handler to our JavaScript.

#### _Update the view_ <a id="update-the-view"></a>

We need to add another link to our view, and set the HTTP method to `DELETE`.

In **app/assets/views/animals/index.html**, add the lines between the comments:

```markup
<% @animals.each do |animal| %>
  <div id=<%= animal.id %>>
    <%= link_to(animal.name, details_path(animal.id), remote: true, :class => "get-animal") %>
    <!-- AJAX DELETE -->
    <%= link_to("Delete Animal", animal_path(animal.id), remote: true, method: :delete, :class => "delete-animal", "data-type" => :json) %>
    <!-- end AJAX DELETE -->
  </div>
<% end %>
<div class="response-text"></div>
```

#### _Reconfigure the controller_ <a id="reconfigure-the-controller"></a>

In **app/controllers/animals\_controller.rb**, change the `destroy` action send back the deleted animal record as JSON \(so we can delete the DOM elements relating to that record\).

```ruby
class AnimalsController < ApplicationController
  # ...
  def destroy
    @animal.destroy
    respond_to do |format|
      format.html { redirect_to animals_url, notice: 'Animal was successfully destroyed.' }
      format.json { render :json => @animal }
    end
  end
  # ...
end
```

#### _Set up the AJAX response handler_ <a id="set-up-the-ajax-response-handler-1"></a>

In **app/assets/javascript/animals.js**, add the following:

```javascript
$(".delete-animal").on("ajax:success", function(event, data, status, xhr) {
  $("#" + data.id).remove();
  $(".response-text").empty().append("Animal deleted");
}).on("ajax:error", function() {
  $error = $("<p>").text("An error occurred. That's all we know.");
  $(".response-text").empty().append($error);
});
```

This will find the element with the ID returned in the ID property of the `data` response and remove it from the DOM, as well as displaying a notice to the user that an Animal was successfully deleted.

### Update records in the database in response to `POST`/`PUT` sent via AJAX <a id="update-records-in-the-database-in-response-to-post-put-sent-via-ajax"></a>

Let's build on the work we've done so far by allowing users to update records without the page being reloaded \(ie, without a new HTML page being sent to the browser\). This follows a similar approach to the `DELETE` request above.

Let's start by changing the **app/views/animals/index.html.erb** to include forms that will allow users to change the `color` attribute of animal records from that view.

Submitting one of these forms will send a `PUT/PATCH` request to the controller to update a particular animal record \(since it already exists\).

```markup
<% @animals.each do |animal| %>
  <div id= <%= animal.id %> >
    <%= link_to(animal.name, details_path(animal.id), remote: true, :class => "get-animal") %>
    <%= link_to("Delete Animal", animal_path(animal.id), remote: true, method: :delete, :class => "delete-animal", "data-type" => :json) %>
    <!-- AJAX PUT/PATCH  -->
    <%= form_for(animal, remote: true, format: :json) do |f| %>
      <%= f.label :color %>
      <%= f.text_field :color, :value => animal.color %>
      <%= f.submit "update" %>
    <% end %>
    <!-- end AJAX PUT/PATCH -->
  </div>
<% end %>
<div class="response-text"></div>
```

#### _Reconfigure the controller_ <a id="reconfigure-the-controller-1"></a>

For this, we're going to change the `update` method so that it just returns a very basic success/failure response \(since we don't really need to retrieve anything from the database\).

Our AJAX response handler is expecting JSON to be returned by the controller, so we do need to send back a JSON object of some kind, but we're just going to send back an object with HTTP status codes only \(`:ok` representing a successful `200` HTTP status, `:unprocessable_entity` representing a failed `422` HTTP status\).

This isn't really necessary, but it's worth checking out the different kinds of responses your controllers can send.

```ruby
def update
  respond_to do |format|
    if @animal.update(animal_params)
      format.html { redirect_to @animal, notice: 'Animal was successfully updated.' }
      format.json { render :json => {status: :ok} }
    else
      format.html { render :edit }
      format.json { render :json => {status: :unprocessable_entity} }
    end
  end
end
```

#### _Set up the AJAX response handler_ <a id="set-up-the-ajax-response-handler-2"></a>

This is going to look pretty similar to our AJAX handler for the `DELETE` request. All this will do is render some text on the page indicating whether the update was successful or not.

Rails is going to generate our forms with the class of "edit\_animal". We're going to add the AJAX event listener to the form itself - this says "listen for any forms with this class being submitted", which is pretty handy \(our listener is bound to the form, and is listening for the submit event, rather than being bound to an click even on an arbitrary button\).

```javascript
$(".edit_animal").on("ajax:success", function(event, data, status, xhr) {
  $(".response-text").empty().append("Animal updated");
}).on("ajax:error", function() {
  $error = $("<p>").text("An error occurred. That's all we know.");
  $(".response-text").empty().append($error);
});
```

### _AJAX on Rails - Further Reading_ <a id="ajax-on-rails-further-reading"></a>

* ​[Rails Guides - Working with JavaScript in Rails](http://guides.rubyonrails.org//v4.2/working_with_javascript_in_rails.html)​
* ​[Rails Guides - Layouts and Rendering in Rails](http://guides.rubyonrails.org/v4.2/layouts_and_rendering.html)​
* ​[API Dock - ActiveModel JSON Serializers](http://api.rubyonrails.org/classes/ActiveModel/Serializers/JSON.html)​



## `Elm-lang`

\`\`[`elm`](https://elm-lang.org/) `- A delightful language for reliable webapps`

#### `Install Elm Globally`

```bash
npm install -g elm
```

#### To view elm code in the bash terminal

```bash
elm repl
```

#### Create .elm files into your project folder

```bash
touch file.elm
```

#### Initialize the Elm project

```bash
elm init
```

#### Start the elm server

```text
elm reactor
```

### Tonight's Homework

* apply what you learned in the MyGA section on Big O Notation to these scraps of code [https://gist.github.com/wofockham/89ef45813bd0f0c31d2f82e22a7553a0](https://gist.github.com/wofockham/89ef45813bd0f0c31d2f82e22a7553a0)

#### Further Reading

* Daisy's slides [https://docs.google.com/presentation/d/10useVTXp9dbreHH0vQpMgDPHezd9itlmshqroYlyw5s/](https://docs.google.com/presentation/d/10useVTXp9dbreHH0vQpMgDPHezd9itlmshqroYlyw5s/)

