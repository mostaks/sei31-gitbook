---
description: Project Proposals Tomorrow...!!!
---

# Day 03

What we covered today:

* Ruby on Rails - Migrations
* Ruby on Rails - Generators
* Ruby on Rails - Associations I
* Ruby On Rails - Basic Setup Version 2

**Warmup**

* [Roman Numerals](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week05/day03_roman_numerals)

#### Classwork

* [MoMA](https://github.com/wofockham/sei-31/tree/master/08-rails/moma)

## Ruby on Rails - Migrations

Migrations are a way to modify your database schema using Ruby code. Using migrations, we can:

* Avoid writing SQL by hand;
* Change our database schema progressively over time;
* Roll back to earlier versions of our database schema;
* Rebuild our database schema from scratch at any time.

Note: Generally, we use migrations for changes to the database schema, not the data in a database; ie, to modify the tables in the database, not to modify the records in the database.

There are a few terminal commands that will generate a migration. These include:

```bash
rails generate model Kitten
rails generate migration add_species_to_kitten
```

Migrations are stored in the db/migrate directory and the filenames are prefixed with a timestamp representing the actual time the migration file was generated, eg: **20160706022128\_create\_kittens.rb**. Rails runs migrations according to the order of their timestamps, so it's important that you do NOT edit the filename of a migration.

We most commonly use migrations to:

1. Create a table in our schema;
2. Modify a table in our schema.

### **Migrations to create a table**

Creating a model using `rails generate model` will create a migration for creating the table for that model:

```bash
rails generate model Kitten
# => create    db/migrate/20160706031227_create_kittens.rb
```

The migration file will look something like this:

```ruby
class CreateKittens < ActiveRecord::Migration
  def change
    create_table :kittens do |t|

      t.timestamps null: false
    end
  end
end
```

In its most basic form, we would then add columns to that table...

```ruby
class CreateKittens < ActiveRecord::Migration
  def change
    create_table :kittens do |t|
      t.string :name
      t.string :name
      t.integer :age
      t.timestamps null: false
    end
  end
end
```

Once we have filled out the migration, we run the migrations - this will run all migrations that have yet to be run, in order according to their timestamps:

```bash
rails db:migrate
```

It's important to check that the migration was successful - ideally, the terminal response should be something like this:

```bash
== 20160706031227 CreateKittens: migrating ====================================
-- create_table(:kittens)
```

If not, there was a problem with your migration! Read the terminal response and try again.

### **Migrations to modify a table**

You can also generate migrations to modify an existing table. If you follow a very specific format in your command, Rails will often be able to generate the necessary code in the migration for you. For example, if the migration name is of the form "AddXXXToYYY" or "RemoveXXXFromYYY" and is followed by a list of column names and types then a migration containing the appropriate add\_column and remove\_column statements will be created.

```bash
rails generate migration AddBreedToKittens breed:string
```

This will generate a migration which can be run without editing the migration itself at all:

```ruby
class AddBreedToKittens < ActiveRecord::Migration
  def change
    add_column :kittens, :breed, :string
  end
end
```

Otherwise, you can generate a migration \(with any name you like\) that you need to complete yourself:

```bash
rails generate migration ChangesToKittens
```

This will generate a migration which needs to be edited before running the migration:

```ruby
class ChangesToKittens < ActiveRecord::Migration
  def change
  end
end
```

### **Bad migrations**

As a general rule, you should not edit or delete a migration once it has been run. This is likely to affect the integrity of your migration history, and cause you a _lot_ of headaches down the line. If you've run a migration and want to change it, the best approach is to write another migration that corrects the issue in your schema.

If you need to delete a migration, you _can_ roll back the migration \(by running `rails db:rollback` until you see that the problematic migration has been reversed\), then edit/edit the migration before running `rails db:create` again. You still need to be careful that none of the subsequent migrations depended on the migration you have edited/deleted.

**Ruby on Rails - Migrations - Further Readings**

* [Rails Guides - ActiveRecord Migrations](http://guides.rubyonrails.org/v4.2/active_record_migrations.html)

## Ruby on Rails - Generators

```ruby
rails generate controller ControllerName list of actions
rails generate controller Greetings index create
rails g controller Greetings index create
# THIS WILL CREATE VIEWS, JS, CSS AND ACTIONS IN CONTROLLERS (PLUS TESTS)

rails g model ModelName field:type
rails g model Painting name:string year:date
# THIS WILL CREATE MODELS, MIGRATIONS, AND TESTS

rails g scaffold ModelName field:type field:type
rails generate scaffold Painting name:string year:date
# THIS WILL CREATE EVERYTHING
```

### Ruby on Rails - Form Helpers

Form helpers are designed to make working with models much easier compared to using just standard HTML elements by providing a set of methods for creating forms based on your models. Example.

```markup
<%= form_for @artist do |f| %>
  <fieldset>
    <%= f.label :name %>
    <%= f.text_field :name %>
  </fieldset>
  <%= end %>
```

### Ruby on Rails - Strong Parameters

To reduce the chance of bad people trying to insert things into the database, we can use strong parameters to disallow all but the fields we have whitelisted. IN the example below, the `Artist` is being created with only the :name, :nationality, :dob, :period, :image from the :artist field \(table\)

```ruby
  def create
    artist = Artist.create artist_params
    redirect_to artist # GET the show page
  end

  # Strong params: create a whitelist of permitted parameters
  private
  def artist_params
    params.require(:artist).permit(:name, :nationality, :dob, :period, :image)
  end
end
```

### Ruby on Rails - Partial

Best example is that the `new` and `edit` are almost exactly the same. We can move the bulk of the code into `_form.html.erb` which will look like...

```markup
<%= form_for @artist do |f| %>
  <fieldset>
    <%= f.label :name %>
    <%= f.text_field :name %>
  </fieldset>
<%= end %>  
```

In this partial form, it is the `@artist` variable which tells the form how to behave. If `@artist` is populated with data, then we have an edit page, if it is a new empty variable then we have a new page.

To place this partial in it's parent form do this;

```markup
<h1>Edit Artist</h1>
<%= render :partial => 'form' %>
```

## Ruby on Rails - Associations I <a id="ruby-on-rails-associations"></a>

Associations allow you to create relationships between any two models \(that inherit from ActiveRecord::Base\). By declaratively telling Rails that two models have a certain association with each other, we can greatly streamline our code.

Rails supports six types of associations, the two simplest associations are `belongs_to` and `has_many`.

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

### _Ruby on Rails - Associations_ <a id="ruby-on-rails-associations-1"></a>

* ​[Rails Guides - Association Basics](http://guides.rubyonrails.org/v4.2/association_basics.html)​

## Ruby on Rails - Basic Project Setup Ver. 2.0 <a id="ruby-on-rails-basic-project-setup-with-database"></a>

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
      <td style="text-align:left">Create migration file</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left"><code>rails generate migration create_kittens</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">8</td>
      <td style="text-align:left">Open atom</td>
      <td style="text-align:left">Atom</td>
      <td style="text-align:left"><code>atom .</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">9</td>
      <td style="text-align:left">Go to db folder</td>
      <td style="text-align:left">Atom</td>
      <td style="text-align:left">Go to app -&gt; db -&gt; migrate -&gt; [your_created_time]_create_kittens.rb</td>
    </tr>
    <tr>
      <td style="text-align:left">10</td>
      <td style="text-align:left">Add columns to migration file</td>
      <td style="text-align:left">Atom</td>
      <td style="text-align:left">
        <p>An example table:</p>
        <p></p>
        <p><code>def change </code>
        </p>
        <p><code> create_table :kittens do |t|</code>
        </p>
        <p><code>  t.string :name </code>
        </p>
        <p><code>  t.text :image </code>
        </p>
        <p><code>  t.integer :roundness</code>
        </p>
        <p><code> end</code>
        </p>
        <p><code>end</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">11</td>
      <td style="text-align:left">Add model</td>
      <td style="text-align:left">Atom</td>
      <td style="text-align:left">
        <p>Go to app -&gt; models, create a new ruby file called &quot;<code>kitten.rb</code>&quot;.
          Inside the file, create <code>Kitten</code> class:
          <br />
        </p>
        <p><code>class Kitten &lt; ApplicationRecord <br /><br />end</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">12</td>
      <td style="text-align:left">Run migration</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left"><code>rails db:migrate</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">13</td>
      <td style="text-align:left">Check schema.rb</td>
      <td style="text-align:left">Atom</td>
      <td style="text-align:left">Go to app -&gt; db -&gt; schema.rb and check the structure is the same
        as your create_kittens.rb</td>
    </tr>
    <tr>
      <td style="text-align:left">14</td>
      <td style="text-align:left">Check migration status</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left">
        <p>(This step is optional)
          <br />
        </p>
        <p><code>rails db:migrate:status</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">15</td>
      <td style="text-align:left">Edit migration as required</td>
      <td style="text-align:left">/db/migrate/[your_migration].rb</td>
      <td style="text-align:left">Add timestamps, more columns, etc if required.</td>
    </tr>
    <tr>
      <td style="text-align:left">16</td>
      <td style="text-align:left">Create seed data</td>
      <td style="text-align:left">Atom</td>
      <td style="text-align:left">
        <p>Go to app -&gt; db -&gt; seed.rb and create seed data. An example:</p>
        <p></p>
        <p><code>Kitten.create name: &quot;cat1&quot;, image: &quot;https://fillmurray.com/100/100&quot;, roundness: 10</code>
        </p>
        <p><code>Kitten.create name: &quot;cat2&quot;, image: &quot;https://fillmurray.com/120/120&quot;, roundness: 1</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">17</td>
      <td style="text-align:left">Load seed data</td>
      <td style="text-align:left">Termnial</td>
      <td style="text-align:left"><code>rails db:seed</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">18*</td>
      <td style="text-align:left">Create first controller</td>
      <td style="text-align:left">Terminal</td>
      <td style="text-align:left"><code>We will do it manually for now<br />(go to the controller folder and create a controller file called &quot;kittens_controller.rb&quot;)</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">19</td>
      <td style="text-align:left">Add methods to the controller</td>
      <td style="text-align:left">/app/controllers/kittens_controller.rb</td>
      <td style="text-align:left">Add methods for each action</td>
    </tr>
    <tr>
      <td style="text-align:left">20</td>
      <td style="text-align:left">Add views for your actions</td>
      <td style="text-align:left">/app/views/kittens/</td>
      <td style="text-align:left">Add an [action].html.erb file for each action that has an associated view</td>
    </tr>
    <tr>
      <td style="text-align:left">21</td>
      <td style="text-align:left">Configure routes</td>
      <td style="text-align:left">/config/routes.rb</td>
      <td style="text-align:left">
        <p><code>root :to =&gt; &quot;kittens#index&quot;</code> 
        </p>
        <p></p>
        <p><code>get &quot;/kittens/new&quot; =&gt; &quot;kittens#new&quot; </code>
        </p>
        <p>&lt;code&gt;&lt;/code&gt;</p>
        <p><code>post &quot;/kittens&quot; =&gt; &quot;kittens#create&quot;</code>
        </p>
        <p>&lt;code&gt;&lt;/code&gt;</p>
        <p><code>get &quot;/kittens&quot; =&gt; &quot;kittens#index&quot;</code>
        </p>
        <p>&lt;code&gt;&lt;/code&gt;</p>
        <p><code>get &quot;/kittens/:id&quot; =&gt; &quot;kittens#show&quot;, as: &quot;kittens&quot;</code>
        </p>
        <p>&lt;code&gt;&lt;/code&gt;</p>
        <p><code>get &quot;/kittens/:id/edit&quot; =&gt; &quot;kittens#edit&quot;, as: &quot;kitten_edit&quot; </code>
        </p>
        <p><code>post &quot;/kittens/:id&quot; =&gt; &quot;kittens#update&quot;</code>
        </p>
        <p>&lt;code&gt;&lt;/code&gt;</p>
        <p><code>get &quot;/kittens/:id/delete&quot; =&gt; &quot;kittens#delete&quot;, as: &quot;kitten_destroy&quot;</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">22</td>
      <td style="text-align:left">Repeat for other models</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">Repeat steps 7-22 (except step 15 &amp; 16) for each model</td>
    </tr>
  </tbody>
</table>\* You can specify the actions for which you would like Rails to create views and methods when generating the controller by adding them to the end of the command - eg `rails generate controller Kittens index show edit new`. This will create those views, and the placeholder methods in your controller, but it also generates a bunch of routes that you will probably want to delete.

## **Homework**

* build a CRUD system with associations 
* plan your project 1 pitch for tomorrow afternoon
* Read:
  * [Rails Guides - Active Record Migrations](http://guides.rubyonrails.org/v4.2/active_record_migrations.html)
  * [Rails Guides - Active Record Associations](http://guides.rubyonrails.org/v4.2/association_basics.html)
  * [https://guides.rubyonrails.org/form\_helpers.html\#dealing-with-model-objects](https://guides.rubyonrails.org/form_helpers.html#dealing-with-model-objects)
  * [https://guides.rubyonrails.org/action\_controller\_overview.html\#strong-parameters](https://guides.rubyonrails.org/action_controller_overview.html#strong-parameters)
  * [https://guides.rubyonrails.org/routing.html\#resource-routing-the-rails-default](https://guides.rubyonrails.org/routing.html#resource-routing-the-rails-default)
  * [https://guides.rubyonrails.org/command\_line.html\#rails-generate](https://guides.rubyonrails.org/command_line.html#rails-generate)

