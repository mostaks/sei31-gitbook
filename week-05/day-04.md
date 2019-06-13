---
description: Enjoy your long weekend and be extra vocal on Slack this weekend!
---

# Day 04

What we covered today:

* PostgresQL Install
* Ruby on Rails - Associations II
* Ruby on Rails - User login:
  * Ruby on Rails - Authentication
  * Ruby on Rails - Authorisation
  * Ruby on Rails - Sessions
* Ruby on Rails - Basic Project Setup \(with multiple models\)

## PostgreSQL - Install

**Installing PostgreSQL**

1. [Download Postgres](http://www.postgresapp.com/).
2. Drag **Postgres** from your Downloads folder to your Applications folder.
3. Configure your $PATH to allow access to Postgres CLI tools
   1. Open your bash profile.

      ```bash
      atom ~/.bash_profile
      ```

   2. Add this line to your bash profile.

      ```bash
      export PATH=$PATH:/Applications/Postgres.app/Contents/Versions/latest/bin
      ```
4. Open an new window in your terminal. This will allow the change to take effect.
5. Install the Postgres Gem - the Ruby interface for PostgreSQL

   ```bash
   gem install pg
   ```

6. Confirm that Postgres has been installed and your $PATH configured properly:

   ```bash
   postgres #This should return:
   # => postgres does not know where to find the server configuration file.
   # => You must specify the --config-file or -D invocation option or set the PGDATA environment variable.
   ```

7. Open Postgres - type 'Postgres' into Spotlight Search and open it up. Close the dialog box that opens up - you should then see the silhouette of an elephant in your Mac's menu bar.

### **Using Postgres**

Generally, you should use PostgreSQL for your Rails applications - it is far more scalable, pretty much just as easy to set up as any other database and works on Heroku \(where we will be hosting our applications during the course\). To start a Rails application with a postgreSQL database, run:

```bash
rails new app_name -T --skipt-git --database=postgresql
```

or simply

```bash
rails new app_name -T --skipt-git -d postgresql
```

## Ruby on Rails - Associations II  <a id="ruby-on-rails-associations"></a>

Associations allow you to create relationships between any two models \(that inherit from ActiveRecord::Base\). By declaratively telling Rails that two models have a certain association with each other, we can greatly streamline our code.

Rails supports six types of associations:

* `belongs_to`
* `has_many`
* `has_one`
* `has_many :through`
* `has_one :through`
* `has_and_belongs_to_many`

The two simplest associations are `belongs_to` and `has_many`.

### `belongs_to` <a id="belongs_to"></a>

A belongs\_to association sets up a one-to-one connection with another model, such that each instance of the declaring model "belongs to" one instance of the other model. For example, if your application includes customers and orders, and each order can be assigned to exactly one customer, you'd declare the order model this way:

```ruby
  class Order < ActiveRecord::Base    
    belongs_to :customer, optional :true  
  end
```

A few things to note about `belongs_to` associations:

* The singularized form of the 'other' model \(`customer`\) _must_ be used for the association to work.
* The `order` table must also include the `customer_id` for the customer records to which it belongs.
* A `belongs_to` association often has a reciprocal `has_one` or `has_many` association on the other model.

### `has_many` <a id="has_many"></a>

A has\_many association indicates a one-to-many connection with another model. You'll often find this association on the "other side" of a belongs\_to association. This association indicates that each instance of the model has zero or more instances of another model. For example, in an application containing customers and orders, the customer model could be declared like this:

```ruby
class Customer < ActiveRecord::Base  
  has_many :orders
end
```

A few things to note about `has_many` associations:

* The pluralized form of the 'other' model \(`orders`\) _must_ be used for the association to work.
* A `has_many` association often has a reciprocal `belongs_to` association on the other model.
* The model with the `has_many` association doesn't store the 'foreign key' of the related model. That goes on the model with the `belongs_to` association.

### `has_one` <a id="has_one"></a>

A `has_one` association also sets up a one-to-one connection with another model, but with somewhat different semantics \(and consequences\). This association indicates that each instance of a model contains or possesses one instance of another model.

```ruby
class User < ActiveRecord::Base  
  has_one :account
end
```

### `has_many :through` <a id="has_many-through"></a>

A `has_many :through` association is often used to set up a many-to-many connection with another model. This association indicates that the declaring model can be matched with zero or more instances of another model by proceeding through a third model.

```ruby
class Doctor < ActiveRecord::Base  
  has_many :appointments  
  has_many :patients, through: :appointments
end​

class Appointment < ActiveRecord::Base  
  belongs_to :doctor  
  belongs_to :patient
end

​class Patient < ActiveRecord::Base  
  has_many :appointments  
  has_many :doctors, through: :appointments
end
```

### `has_one :through` <a id="has_one-through"></a>

A `has_one :through` association sets up a one-to-one connection with another model. This association indicates that the declaring model can be matched with one instance of another model by proceeding through a third model.

```ruby
class Supplier < ActiveRecord::Base  
  has_one :account  
  has_one :account_history, through: :account
end

​class Account < ActiveRecord::Base  
  belongs_to :supplier  
  has_one :account_history
end

​class AccountHistory < ActiveRecord::Base  
  belongs_to :account
end
```

### `has_and_belongs_to_many` <a id="has_and_belongs_to_many"></a>

A has\_and\_belongs\_to\_many association creates a direct many-to-many connection with another model, with no intervening model. This association is often abbreviated to 'HABTM'.

```ruby
class Assembly < ActiveRecord::Base  
  has_and_belongs_to_many :parts
end​

class Part < ActiveRecord::Base  
  has_and_belongs_to_many :assemblies
end
```

### _Ruby on Rails - Associations_ <a id="ruby-on-rails-associations-1"></a>

* ​[Rails Guides - Association Basics](http://guides.rubyonrails.org/v4.2/association_basics.html)​

## Authentication <a id="authentication"></a>

### Authentication v Authorization <a id="authentication-v-authorization"></a>

* **Authentication** is the process of determining who a user is - it is about identity.
* **Authorization** is the process of declaring what a user is allowed to do - it is about permissions.

The term 'authentication' is often loosely applied to the end-to-end process of signing-up, logging-in and signing-out a user.

### Authentication in Rails <a id="authentication-in-rails"></a>

Authentication is rough, and rarely \(if ever\) will you be asked to build your own authentication system. There are plenty of gems that will do that sort of thing, but if you want to do it for yourself - there are a bunch of things you need to do.

First things first - we don't store passwords in plain text, we store it in something called a `password_digest`. This is an encrypted password.

Let's create our User model!!

### User model and users database table <a id="user-model-and-users-database-table"></a>

```bash
# Create our User model and the migration for creating the users table in the database.
rails generate model User name:string password_digest:string age:integer email:string
​
# Run the migration generated to create the users table in the database.
rails db:migrate
```

### Password encryption <a id="password-encryption"></a>

Now, we need to use our Gemfile to get something that does the password encryption. We normally use [bcrypt](https://github.com/codahale/bcrypt-ruby) for this. Let's add it!

```ruby
gem 'bcrypt'
```

In our User model, we need to add the following line:

```ruby
has_secure_password
```

Including this method in our user model adds methods to set and authenticate encrypted passwords. In order for the `has_secure_password` method to work, your database table _must_ have a `password_digest` column.

`has_secure_password` adds the following validations to your model:

* A password must be present when a User object is created;
* A password must be &lt;= 72 characters long;
* The password and password\_confirmation\* must match.

\* The `password == password_confirmation` validation check will only be triggered if a password\_confirmation field is present. For forms that do not require this validation \(eg, sign-in\), just leave the password\_confirmation field out altogether.

### Forms <a id="forms"></a>

Even though the users table in our database has a `password_digest` column, any forms relating to sign-up or sign-in _still_ need to use `password` and `password_confirmation` parameters. A user doesn't enter their `password_digest` directly - Rails knows that the model "has secure password", and when bcrypt sees a `password` and `password_confirmation`, it knows what to do with it.

### Interlude - HTTP 'state' <a id="interlude-http-state"></a>

Now we can authenticate a user based on their credentials, including an encrypted password, but before we get into authorization \(what a user can _do_, having been _authenticated_\), we need a way to be able to remember who an authenticated user is.

HTTP is stateless, so either the browser or the application \(or both\) need to 'remember' who a user is. To do this, we're going to store their identity in something called the **session hash**\*. Without this, a user would need to actively prove their identity every single time they send a request to the server.

\*The session hash is not actually a hash, it just behaves like one.

## Authorisation <a id="authorisation"></a>

Now that we have set up the authentication system, let's work on authorisation - specifying what actions we will allow users to perform.

Let's say we want most actions in our application to be restricted to authenticated users - users who have signed in using valid credentials, and whose user\_id is now stored in the sessions hash \(thanks to our `session#create` action\).

Since this is not going to be limited to a particular controller, we'll put this code in our application\_controller, from which all other controllers inherit. So, in app/controllers/application\_controller.rb:

```ruby
class ApplicationController < ActionController::Base
​
  # Before any action is performed, call the fetch_user method.
  before_action :fetch_user
​
  private
​
  def fetch_user
    # Search for a user by their user id if we can find one in the session hash.
    if session[:user_id].present?
      @current_user = User.find_by :id => session[:user_id]
​
      # Clear out the session user_id if no user is found.
      session[:user_id] = nil unless @current_user
    end
  end
​
​
  def authorize_user
    redirect_to root_path unless @current_user.present?
  end
end

```

Now we have a `@current_user` variable which will be available whenever a session includes a `user_id`. We can use the presence of this variable to perform simple authorisation tasks.

We also have an `authorise_user` method, which will redirect a user to the root\_path if that `@current_user` variable is not present. We probably don't want to create a `before_action` for this method in our application controller, since there are going to be a number of actions we want unauthenticated users to be able to do, like access the homepage, sign-up, sign-in, etc.

Instead, we'll call that method on a controller-by-controller basis.

```ruby
class UsersController < ApplicationController
  before_action :authorise_user, :except => [:index]
end
```

The `:except` method allows us to limit the scope of this authorisation method to all actions within the Users controller _except_ a particular action or actions - in this case, the index action. This means that unauthenticated users will still be able to view the Users index.

We can extend this pattern to other types of authorisation. For example, if our users table has an `admin` column with the data type `boolean`, we could restrict particular actions to authenticated admin users. For example:

```ruby
class AdminController < ApplicationController
  before_action :authorise_admin
​
  def burn_all_bridges
    [User, Song, Mixtape, Artist, Genre, Album].each do |table|
      table.destroy_all
    end
  end
​
  private
  def authorise_admin
    redirect_to root_path unless @current_user.present? && @current_user.admin
  end
end
```

### Sessions <a id="sessions"></a>

When a user visits a Rails site \(or rather, when a client sends a request to a Rails server\), the application checks whether the client includes a cookie containing a valid session ID for the application. If none exists \(ie, if this is a new user, or they are not logged in\), the app will create a new session on the application side, and store that in an encrypted cookie on the client-side.

Two parts of the session that are pivotal to authentication and authorization are the **session hash** and **flash hash**:

#### _Session hash_ <a id="session-hash"></a>

The session hash:

* Is a collection of key/value pairs;
* Includes a session\_id. This ID is:
  * A 32 character string of random characters;
  * Included in every cookie sent from the application to the client's browser;
  * Sent to the server with every request from the client;
* Is unique to each user \(and unique to a particular user's 'session'\);
* Is private between the user and the server.

We can store a \(small\) bunch of information in the session hash, including an authenticated user's `user_id` - this is the key to effective authorization in Rails.

#### _Flash hash_ <a id="flash-hash"></a>

The flash hash is a special part of the session that is cleared after each HTTP request - this means that values stored there will only be available in the next request, which is useful for passing error messages etc \(eg, if a user is not authorized to access a particular action\).

The flash hash:

* Is a collection of key/value pairs;
* Has arbitrary keys, but the convention is to use:
  * `flash[:notice]` or `flash[:success]` for storing success messages;
  * `flash[:error]` for storing error messages.
* Will be available for: 1. the current request; and 2. the subsequent request \(unless `flash.now` is used\)

  before it is destroyed \(unless `flash.keep` is used\).

A few helpful tips for dealing with the flash hash:

* If you want to set a flash that will only be available for the current action - and _not_ be available to the subsequent action - use the `flash.now` method.
* If you want to keep a flash for more than one controller action, use the `flash.keep` method in the action which should carry the flash on to the next action.

Example usage:

```ruby
def create
  user = User.create(user_params)
  if @user.save
    flash[:notice] = 'User was successfully created.'
    redirect_to user_path(user)
  else
    flash.now[:error] = 'Could not create user.'
    render 'new'
  end
end
```

Below is a pattern for storing a user's `user_id` to their session hash when they sign-in, and to track this for authorization within the application.

#### _Create a session controller_ <a id="create-a-session-controller"></a>

Our session controller has no associated model and only requires three actions - new, create and destroy.

```bash
rails generate controller Session new create destroy
```

Note: This will create two views that we don't need. Delete those.

```bash
rm app/views/session/create.html.erb
rm app/views/session/destroy.html.erb
```

Note: This will also have create several routes that we don't want. Delete the following routes from your config/routes.rb file:

```ruby
get 'session/new'
get 'session/create'
get 'session/destroy'.
```

#### _Setup login routes_ <a id="setup-login-routes"></a>

Add the following routes to your config/routes.rb file:

```ruby
root :to => 'pages#home'              # Replace this with whatever you want your root_path to be.
                                      # This path is where unauthorized users will be redirected_to.
get '/login' => 'session#new'         # This will be our sign-in page.
post '/login' => 'session#create'     # This will be the path to which the sign-in form is posted
delete '/login' => 'session#destroy'  # This will be the path users use to log-out.
```

#### _Setup session controller actions_ <a id="setup-session-controller-actions"></a>

We want to use our session controller to handle login and logout. If a user logs in with a valid username & password combination, their user\_id will be stored in the session hash. If a user logs out, the user\_id will be removed from the session hash.

In your app/controllers/session\_controller.rb file:

```ruby
class SessionController < ApplicationController
​
​
  def new
  # This is the action for user login. The view will have the login form template.
  end
​
  def create
  # This is the action to which the login form post request is posted. It will add the 
  #user's id to the session hash, which will be used for authentication and 
  #authorization throughout the session.
    user = User.find_by :email => params[:email]
    if user.present? && user.authenticate(params[:password])
      # If a user record with the entered in the form is present AND the user is 
      #authenticated (using bcrypt's authenticate method and the password entered in 
      #the form), store their id in the session hash and redirect them to the root 
      #path.
      session[:user_id] = user.id
      flash[:notice] = "User created!"
      redirect_to root_path
    else
      # If the user cannot be authenticated, redirect them to the login_path.
      flash[:error] = "User could not be created!"
      redirect_to login_path
    end
  end
​
  # This is the action to which the user sign-out delete request is posted.
  def destroy
    session[:user_id] = nil
    redirect_to root_path
  end
end
```

#### _Add sign-up / log-in / sign-out links to our layout_ <a id="add-sign-up-log-in-sign-out-links-to-our-layout"></a>

You'll probably won't to wrap this in some conditional logic once you have your `fetch_user` method set up \(see Authorization, below\), but these links in your app/views/layouts/application.html.erb layout will allow users to sign-up, log in and sign-out:

```markup
<ul>
  <li><%= link_to("Sign Up", new_user_path %>) %></li>
  <li><%= link_to("Sign In", login_path %>) %></li>
  <li><%= link_to("Log Out", login_path, :method => :delete %>) %></li>
</ul>
```

* ​[The Buckner Life - Simple Authentication with Bcrypt](https://gist.github.com/thebucknerlife/10090014) - this is a great, step-by-step Bcrypt tutorial.
* ​[Rails Guides - Security Guide](http://guides.rubyonrails.org/security.html)​

## Ruby on Rails - Basic Project Setup \(with multiple models\) <a id="ruby-on-rails-basic-project-setup-with-database"></a>

Treat this as a really rough guide, and definitely don't always follow it - you'll figure out your approach soon. This is a basic approach you might follow when setting up a new Rails project:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Step</th>
      <th style="text-align:left">What</th>
      <th style="text-align:left">Where</th>
      <th style="text-align:left">How</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">1</td>
      <td style="text-align:left">Planning</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">Pen &amp; paper</td>
    </tr>
    <tr>
      <td style="text-align:left">2</td>
      <td style="text-align:left">Create a new Rails app</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left"><code>rails new kitten_party --skip-git --skip-turbolinks -d postgresql</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">3</td>
      <td style="text-align:left">Move into the new app&apos;s dir</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left"><code>cd kitten_party</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">4</td>
      <td style="text-align:left">Add development Gems</td>
      <td style="text-align:left">/Gemfile</td>
      <td style="text-align:left"><code>gem &quot;pry-rails&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">5</td>
      <td style="text-align:left">Bundle Gems</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left"><code>bundle</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">6</td>
      <td style="text-align:left">Create database</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left"><code>rails db:create</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">7</td>
      <td style="text-align:left">Generate first model</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left"><code>rails generate model Kitten name:string age:integer</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">8</td>
      <td style="text-align:left">Edit migration as required</td>
      <td style="text-align:left">/db/migrate/[your_migration].rb</td>
      <td style="text-align:left">Add timestamps, more columns, etc if required.</td>
    </tr>
    <tr>
      <td style="text-align:left">9</td>
      <td style="text-align:left">Generate first controller</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left">
        <p>E.g.</p>
        <p><code>rails generate controller Kittens index show edit new </code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">10</td>
      <td style="text-align:left">Add methods to the controller</td>
      <td style="text-align:left">/app/controllers/kittens_controller.rb</td>
      <td style="text-align:left">Add methods for each action</td>
    </tr>
    <tr>
      <td style="text-align:left">11</td>
      <td style="text-align:left">Add views for your actions</td>
      <td style="text-align:left">/app/views/kittens/</td>
      <td style="text-align:left">Add an [action].html.erb file for each action that has an associated view</td>
    </tr>
    <tr>
      <td style="text-align:left">12</td>
      <td style="text-align:left">Configure routes</td>
      <td style="text-align:left">/config/routes.rb</td>
      <td style="text-align:left"><code>root :to =&gt; &quot;kittens#index&quot;</code>  <code>resources :kittens</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">13</td>
      <td style="text-align:left">Repeat for other models</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">Repeat steps 7-12 for each model</td>
    </tr>
  </tbody>
</table>### Homework <a id="homework"></a>

* Start Project1 and don't panic!!!

###  <a id="undefined"></a>

