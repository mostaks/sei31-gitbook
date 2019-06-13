---
description: 'Homework: CRUDdy Mountains or Oceans'
---

# Day 02

What we covered today:

* Ruby on Rails - Conventions
* Ruby on Rails - Rails in the Terminal
* Ruby on Rails - Basic Project Setup \(with database\)

### Warmup <a id="warmup"></a>

* [​Robot factory](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week05/day02_robot_factory)​​

### Classwork <a id="classwork"></a>

* [Solar System](https://github.com/wofockham/wdi-30/tree/master/08-rails/solar_system)
* [moma](https://github.com/wofockham/sei-31/tree/master/08-rails/moma)

## Ruby on Rails - Conventions

### **Naming conventions**

### **Models**

Models are singular, with a capitalized class name and inherit from ActiveRecord::Base. The Model files are located in `app/models`, have a singular, snake\_cased file name and an extension of .rb

### **Controllers**

Controllers are plural, with a PascalCase class name and inherit from ApplicationController. The Controller files are located in `app/controllers`, have a plural, snake\_cased file name ending in `controller` and an extension of `.rb`.

### **Views**

Views are a bit more difficult. They reside in a folder in `app/views`, the name of that folder comes from the associated controller and is plural and snake\_cased.  
The name of the actual file is the name of the associated action \(or method\), and have an extension of `.html.erb`. They don't inherit from anything.

| Type | File Name | Location | Class Name | Inherits From |
| :--- | :--- | :--- | :--- | :--- |
| Model | planet.rb | app/models | Planet | ActiveRecord::Base |
| Controllers | planets\_controller.rb | app/controllers | PlanetsController | ApplicationController |
| Views | index.html.erb | app/views/planets | N/A | N/A |

## Ruby on Rails - Rails in the Terminal

There are a few commands that are absolutely essential for Rails development. The more you know them, the better it is!

At its most basic:

* `rails new` - Generate a new application
* `rails server` - Runs the server
* `rails generate` - Generate a whole heap of things within an application
* `rails console` - Opens up a console
* `rails dbconsole` - Opens up a direct connection to SQL
* `bundle` - Install gems and their dependencies

Running any command with `-h` or `--help` at the end will show you the documentation for that particular command. But the real power comes from...

### **Customization and Automation!**

**rails new**

```bash
rails new app_name
rails new app_name -T # Skips the Test Suite
rails new app_name --database=postgresql # Specifies the Database (changes it from sqlite3)
rails new app_name -d postgresql
```

**rails server**

```bash
rails server
rails s # Shorthand for rails server
rails server -p 3001 # Specifies another port (have multiple servers at once!)
rails server -e production # Changes the state of the application (different gem sets etc. - don't worry about this one)
```

**rails destroy**

If you have run a `rails generate` command and want to reverse all the changes it made, including destroying all the files it created, you can use the `rails destroy` command.

```bash
rails destroy controller Kittens
rails destroy model Kitten
rails destroy scaffold Kittens
```

You can also include the `-p` flag \("pretend"\) to see what files _would be deleted_ by the command without actually deleting them!

```bash
rails destroy controller Kittens -p
# remove  app/controllers/kittens_controller.rb
# invoke  erb
# remove    app/views/kittens
# invoke  test_unit
# remove    test/controllers/kittens_controller_test.rb
# invoke  helper
# remove    app/helpers/kittens_helper.rb
# invoke    test_unit
# invoke  assets
# invoke    coffee
# remove      app/assets/javascripts/kittens.coffee
# invoke    scss
# remove      app/assets/stylesheets/kittens.scss
```

**rails console**

```bash
rails console # OPENS UP YOUR RAILS APP IN PRY OR IRB
rails c # SHORTHAND

rails console staging # OPENS UP A SPECIFIC ENVIRONMENT

rails console --sandbox # CAN'T MAKE ANY ACTUAL CHANGES
```

**rails dbconsole**

```bash
rails dbconsole # OPENS UP A DIRECT CONNECTION TO YOUR DATABAS
rails db # SHORTHAND
```

**bundle**

```bash
bundle install # INSTALLS GEM AND DEPENDENCIES
bundle # SHORTHAND
```

**rails**

This is crazy powerful and does a million things, but here are some of the more important ones that you might need to know. We will go into this in a lot more detail though!

```bash
rails --tasks # Lists everything it can do

rails about # Lists everything about your Rails app

rails db:drop # DROPS THE DATABASE
rails db:create # CREATES THE DATABASE
rails db:migrate # MIGRATES TABLES INTO THE DATABASE (FROM db/migrations)
rails db:rollback # GOES BACK ONE STEP IN THE DATABASE (BACK ONE MIGRATION)

rails routes # LIST ALL OF YOUR ROUTES

rails stats # LINES OF CODE ETC.

rails notes # SEE HERE - http://guides.rubyonrails.org/v4.2/command_line.html#notes
```

**Ruby on Rails - Rails in the Terminal - Further Readings**

* [Rails Guides - The Rails Command Line](http://guides.rubyonrails.org/v4.2/command_line.html)

## Ruby on Rails - Basic Project Setup \(with database\) <a id="ruby-on-rails-basic-project-setup-with-database"></a>

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
      <td style="text-align:left"><code>rails new kitten_party</code>
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
      <td style="text-align:left">Open atom</td>
      <td style="text-align:left">Atom</td>
      <td style="text-align:left"><code>atom .</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">8</td>
      <td style="text-align:left">Go to db folder</td>
      <td style="text-align:left">Atom</td>
      <td style="text-align:left">Go to db -&gt; create <code>create_kittens.sql</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">9</td>
      <td style="text-align:left">Create table</td>
      <td style="text-align:left">Atom</td>
      <td style="text-align:left">
        <p>Inside sql file, an example table:</p>
        <p>&#x200B;</p>
        <p><code>CREATE TABLE kittens ( </code>
        </p>
        <p><code>id INTEGER PRIMARY KEY AUTOINCREMENT, </code>
        </p>
        <p><code>name TEXT, </code>
        </p>
        <p><code>image TEXT</code>
        </p>
        <p><code> );</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">10</td>
      <td style="text-align:left">Add sql table into database</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left">
        <p><code>cd db</code>
        </p>
        <p><code>sqlite3 development.sqlite3 &lt; create_kittens.sql</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">11</td>
      <td style="text-align:left">Check schema</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left">
        <p>Run:</p>
        <p><code>rails db</code>
        </p>
        <p><code>..&gt; .schema</code>
        </p>
        <p>Check the structure is the same as your create_kittens.sql</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">12</td>
      <td style="text-align:left">Create seed data</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left">
        <p>Run:</p>
        <p><code>rails console</code>
        </p>
        <p>and create seed data. An example:&#x200B;</p>
        <p><code>k1 = Kitten.new</code>
        </p>
        <p><code>ki.name = &quot;mimi&quot;</code>
        </p>
        <p><code>ki.save</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">13*</td>
      <td style="text-align:left">Create first controller</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left"><code>We will do it manually for now (go to the controller folder and create a controller file called &quot;kittens_controller.rb&quot;)</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">14</td>
      <td style="text-align:left">Add model</td>
      <td style="text-align:left">Atom</td>
      <td style="text-align:left">
        <p>Go to app -&gt; models, create a new ruby file called &quot;<code>kitten.rb</code>&quot;.
          Inside the file, create <code>Kitten</code> class:</p>
        <p><code>class Kitten &lt; ActiveRecord::Base</code>
        </p>
        <p><code><br />end</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">15</td>
      <td style="text-align:left">Add methods to the controller</td>
      <td style="text-align:left">/app/controllers/kittens_controller.rb</td>
      <td style="text-align:left">Add methods for each action</td>
    </tr>
    <tr>
      <td style="text-align:left">16</td>
      <td style="text-align:left">Add views for your actions</td>
      <td style="text-align:left">/app/views/kittens/</td>
      <td style="text-align:left">Add an [action].html.erb file for each action that has an associated view</td>
    </tr>
    <tr>
      <td style="text-align:left">17</td>
      <td style="text-align:left">Configure routes</td>
      <td style="text-align:left">/config/routes.rb</td>
      <td style="text-align:left">
        <p><code>root :to =&gt; &quot;kittens#index&quot;</code>
        </p>
        <p>&#x200B;</p>
        <p><code>get &quot;/kittens/new&quot; =&gt; &quot;kittens#new&quot;</code>
        </p>
        <p><code>&#x200B;</code>
        </p>
        <p><code>post &quot;/kittens&quot; =&gt; &quot;kittens#create&quot;</code>
        </p>
        <p><code>&#x200B;</code>
        </p>
        <p><code>get &quot;/kittens&quot; =&gt; &quot;kittens#index&quot;</code>
        </p>
        <p><code>&#x200B;</code>
        </p>
        <p><code>get &quot;/kittens/:id&quot; =&gt; &quot;kittens#show&quot;, as: &quot;kittens&quot;</code>
        </p>
        <p><code>&#x200B;</code>
        </p>
        <p><code>get &quot;/kittens/:id/edit&quot; =&gt; &quot;kittens#edit&quot;, as: &quot;kitten_edit&quot;</code>
        </p>
        <p><code>post &quot;/kittens/:id&quot; =&gt; &quot;kittens#update&quot;</code>
        </p>
        <p><code>&#x200B;</code>
        </p>
        <p><code>get &quot;/kittens/:id/delete&quot; =&gt; &quot;kittens#delete&quot;, as: &quot;kitten_destroy&quot;</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">18</td>
      <td style="text-align:left">Repeat for other models</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">Repeat steps 8-17 for each model</td>
    </tr>
  </tbody>
</table>\* You can specify the actions for which you would like Rails to create views and methods when generating the controller by adding them to the end of the command - eg `rails generate controller Kittens index show edit new`. This will create those views, and the placeholder methods in your controller, but it also generates a bunch of routes that you will probably want to delete.

## Homework <a id="homework"></a>

* Create a Rails CRUD system for a single model: either Mountain or Ocean. Remember to call your app something other than Mountain or Ocean! Set up the database before you start on the routes. Choose which columns your database will store. Feel free to use helpers like `number_to_human` to present your HTML in the most readable way possible. Add CSS until you've got something you're comfortable presenting tomorrow.

