---
description: >-
  Why did the SQL Programmer get fired as a removalist? Because he was always
  dropping tables
---

# Day 04

What we covered today:

* SQL \(Structured Query Language\)
* Sinatra - CRUD with SQL

**Warmup**

* [Nucleotide](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week04/day04_nucleotide_count)

#### Classwork

* [Class Demo](https://github.com/wofockham/sei-31/tree/master/07-databases)

## SQL

This is a really difficult topic and not one that we expect you to be able to write out - as long as you can get it in terms of principles - it is all good.

When we talk about tables and databases, there are really only 4 tasks that we ever want to perform:

* **Create**
* **Read**
* **Update**
* **Delete**

This is called **CRUD**.

There are a few things to all databases: We have:

* The **database** itself
* Individual **tables** within that database
* Individual **records** within those tables.

Even though a table lives inside a database, we ordinarily start with the file we use to create a table, then build our database using that.

```sql
CREATE TABLE table_name (
    -- Comma seperated list of attributes with a type and a list of options
    column_name DATATYPE
);

CREATE TABLE people (
    id INTEGER PRIMARY KEY,
    first_name TEXT,
    last_name TEXT,
    age INTEGER
);
```

This normally goes in a file with a file name ending with .sql. You might call this person.sql, and obviously we can have lots of these. As many as you want.

This hasn't created the database though, we need to be explicit about that.

`sqlite3 desired_database_name.sqlite3 < add_this_table.sql`

`sqlite3 database.sqlite3 < people.sql`

This line will create the database.sqlite3 file if necessary, and if not - it will just add whatever is defined in the .sql file specified. It imports the details from the .sql into the database.sqlite3.

To make sure this has worked, type in `sqlite3 database.sqlite3` and hit enter in the terminal. This will open up a direct line to the database in the current folder. If you type `.schema` it will show the current tables.

### **Create**

Once we have the table defined, we need to figure out how to actually put records into it.

```sql
INSERT INTO table_name ( comma, separated, columns ) VALUES ( commas_value, separated_value, column_value);

INSERT INTO people (id, first_name, last_name, age) VALUES ( 1, "Bob", "Dobbs", 73 );

-- We don't need to specify the attributes though - we can just use the values, like this...

INSERT INTO people VALUES ( 1, "Bob", "Dobbs", 73 );
```

This is the creation step. If you wrote this in a file, you can import that SQL into the database - `sqlite3 database_name.db < insert_stuff.sql`

### **Read**

This is pretty annoying to write.

```sql
-- SELECT what FROM what_table;
-- SELECT what FROM what_table WHERE options;

SELECT * FROM people; -- this will select all attributes of all records from the person table

SELECT name FROM people; -- only select the name attributes of all the records in the person table

SELECT * FROM people WHERE first_name = "Bob"; -- select all attributes from all records in the person database where the first_name in the record  is "Bob"
```

### **Update**

And this is pretty annoying to write.

```sql
UPDATE table SET attribute_name = attribute_value WHERE attribute_name = attribute_value;

UPDATE people SET first_name = "Joel" WHERE first_name = "Bob";
```

### **Delete**

And _this_ is pretty annoying to write too.

```sql
DELETE FROM what_table WHERE what_attributes = what_value;

DELETE FROM people WHERE first_name = "Bob"; -- Delete all records in the person table where the first_name is equal to "Bob"
```

That is the basics of SQL, for more - see the further readings below. It's all about the principles though; as long as you understand the fact that you need a database to have tables, and tables to have records - that is all good.

**CRUD application overview**

In terms of actual structure of an application, this is the structure of a CRUD application. 7 views for all of this! The \#new and the \#edit are just ways to show the actual form.

| CRUD | HTTP VERBS | URLS | SQL | NAME |
| :--- | :--- | :--- | :--- | :--- |
| CREATE | POST | /butterflies | INSERT | \#create |
|  |  | /butterflies/new |  | \#new |
| READ | GET | /butterflies | SELECT | \#index |
|  | GET | /butterflies/:id | SELECT | \#show |
| UPDATE | POST | /butterflies/:id | UPDATE | \#update |
|  |  | /butterflies/:id/edit |  | \#edit |
| DELETE | \(Delete\) | /butterflies/:id | DELETE | \#delete |

CRUD is the foundation of most applications on the web, it is the thing that powers it! Important to get the principles of it.

**SQL - Further Readings**

* [Learn Code The Hard Way - SQL](http://sql.learncodethehardway.org/book/)

## Sinatra - CRUD with SQL

Now that we've got a basic understanding of Sinatra, SQL and CRUD, we can combine the three to create a basic CRUD app.

**Database connections**

Whenever a request is sent to our web server that requires data to be retrieved from or posted to our database, the server needs to open a connection to the database. When using the **SQLite3 gem**, and assuming our database is stored in a file called **database.sqlite3**, such requests will follow this basic pattern:

### **Open a new database connection**

Create a new connection to the database stored in 'database.sqlite3' using SQLite3 as our database adaptor. This connection is really a Ruby object that is an instance of the SQLite3 gem's `Database`class. Store this connection in a variable called `db`. We will then be calling various methods on this object.

```text
db = SQLite3::Database.new("database.sqlite3")
```

### **Return results as hashes**

By default, SQL will retrieve arrays of results from the database, but we want the results to be returned as hashes.

```text
db.results_as_hash = true
```

### **Write an SQL query**

Write an SQL query and store it in a variable called 'sql'. This step isn't completely necessary - we can pass an SQL query as a string directly into the `execute` method below, but storing the query in a variable keeps things tidy.

```text
sql = "SELECT * FROM animals;"
```

### **Execute the SQL query**

Call the 'execute' method on our database connection object, passing in the SQL query we want to execute. If we're passing the records retrieved to our view, we need to store the response in an instance variable \(using an `@` symbol\).

```text
@all_animals = db.execute(sql)
```

### **Close the database connection**

Once a response has been received, close the connection to the database, since we can only have a limited number of concurrent database connections.

```text
db.close
```

**Further Reading - Database Connections**

* [RubyGems - SQLite3::Database](http://www.rubydoc.info/gems/sqlite3/1.3.11/SQLite3/Database)

## **Routes**

We're going to have a very simple database which just has a single table. We need to create all the routes for CRUD'ing records to that table.

| Action | Purpose | CRUD Action |
| :--- | :--- | :--- |
| Index | Display a list of all the records in the table | READ |
| Show | Display a single record in the table | READ |
| Edit | Display a form for modifying the attributes of a single record in the table | - |
| Update | Update the attributes of a single record in the table | UPDATE |
| New | Display a form for setting the attributes of a new record in the table | - |
| Create | Add a new record to the table | INSERT |
| Delete | Remove a single record from the table | DELETE |

Below is the basic skeleton of a main.rb file to set up a CRUD system for adding records to a table of `animals` in a database.

```ruby

# Require all the gems we need:
require 'sqlite3'           # Sqlite3 for our database
require 'pry'               # Pry for debugging
require 'sinatra'           # Sinatra for our web server
require 'sinatra/reloader'  # Sinatra Reloader so we don't need to keep restarting our server

# GET requests to the root path. No connection to the database required here.
get '/' do
    erb :home
end

# GET requests to the /animals path - this will be the index of all the records in our 'animals' table
get '/animals' do
    # Create a new connection to the 'database.sqlite3' using SQLite3 as our database adaptor, and store this connection in a variable called 'db'
    db = SQLite3::Database.new("database.sqlite3")
    # We want the results to be returned as hashes, rather than as (the default) arrays.
    db.results_as_hash = true
    # Call the 'execute' method on our database connection, passing in the SQL query we want to execute (one which will return all the records in the 'animals' table), and store the response in an instance variable called '@all_animals', which will make this variable available in the view rendered by this route (the animals_index view - see below)
    @all_animals = db.execute "SELECT * FROM animals;"
    # Once a response has been received, close the connection to the database, since we can only have a limited number of concurrent database connections.
    db.close
    # Render the animals_index erb template (this will look for a file called animals_index.erb in our views folder)
    erb :animals_index
end

# GET requests to the /animals/new path. No connection to the database required here - it's just a bunch of form fields.
get '/animals/new' do
    # Render the animals_new erb template (this will look for a file called animals_new.erb in our views folder). This will be the form for setting the attributes for a new animal record.
    erb :animals_new
end

# POST requests to the /animals path - this will add a new record to the 'animals' table in the database, taking the data from the form in the animals_new view
post '/animals' do
    # Create a new connection to the 'database.sqlite3' using SQLite3 as our database adaptor, and store this connection in a variable called 'db'
    db = SQLite3::Database.new("database.sqlite3")
    # We want the results to be returned as hashes, rather than as (the default) arrays.
    db.results_as_hash = true
    # Instead of passing the SQL query text directly in the db.execute method below, let's store the SQL query we want in a variable called 'sql' and pass that in. We don't _have_ to do this, but it's nice to break things down a bit. The :species, :image, and :description keys in the params hash come from the form submitted from the 'new_animal' form - specifically, input elements with 'name' attributes matching those keys (eg input entered into the <input name="species"> field will go into the :species key of the params hash as params[:species] ).
    sql = "INSERT INTO animals (species, image, description) VALUES ('#{params[:species]}', '#{params[:image]}', '#{params[:description]}');"

    # Call the 'execute' method on our database connection, passing in the SQL query we want to execute (one which will add a new record to the animals table in the database).
    db.execute(sql)
    # Once a response has been received, close the connection to the database, since we can only have a limited number of concurrent database connections.
    db.close
    # We don't actually want to render a view here - there's no view that corresponds with a 'create' action - the create action happens on the back end. Instead, once the code above has been run and a new record added to the animals table in the database, we want to redirect the browser to our GET '/animals' route (which displays the animals index).
    redirect "/animals"
end

# GET requests to the /animals/:id/edit path - this will render the form to update the attributes of an existing record in the animals table. We _DO_ need a database connection here, since we want to retrieve the records attributes to display them in the form (so the user knows what they're editing)
get '/animals/:id/edit' do
    # Create a new connection to the 'database.sqlite3' using SQLite3 as our database adaptor, and store this connection in a variable called 'db'
    db = SQLite3::Database.new("database.sqlite3")
    # We want the results to be returned as hashes, rather than as (the default) arrays.
    db.results_as_hash = true
    # Call the 'execute' method on our database connection, passing in the SQL query we want to execute (one which will return a record from the animals table in the database, using the ID in the params hash (which will be whatever's in the :id space in the path /animals/:id/edit)), and store the response in an instance variable called '@animal', which will make this variable available in the view rendered by this route (the animals_edit view, below). We need to select the first record, since the response will be in an array.
    @animal = db.execute( "SELECT * FROM animals WHERE id == #{params[:id]};" ).first
    # Once a response has been received, close the connection to the database, since we can only have a limited number of concurrent database connections.
    db.close
    # Render the animals_edit erb template (this will look for a file called animals_edit.erb in our views folder)
    erb :animals_edit
end

# POST requests to the /animals/:id path - this will take attributes from the edit form (which will be available in the params hash) and update the record with the id matching the :id key in the params hash
post '/animals/:id' do
    # Create a new connection to the 'database.sqlite3' using SQLite3 as our database adaptor, and store this connection in a variable called 'db'
    db = SQLite3::Database.new("database.sqlite3")
    # We want the results to be returned as hashes, rather than as (the default) arrays.
    db.results_as_hash = true
    # Save the sql query we want to pass into the execute method of the database connection in a variable (just to make our code a bit easier to read)
    sql = "UPDATE animals SET species = '#{params[:species]}', image = '#{params[:image]}', description = '#{params[:description]}' WHERE id == #{params[:id]};"
    # Call the execute method on the open database connection, passing in the SQL query to create a new record using data in the params hash
    db.execute(sql)
    # Once a response has been received, close the connection to the database, since we can only have a limited number of concurrent database connections.
    db.close
    # We don't actually want to render a view here - there's no view that corresponds with a 'update' - the update action happens on the back end. Instead, once the code above has been run and the relevant record has been updated in the animals table in the database, we want to redirect the browser to our GET '/animals/:id' route (which displays the animal's view page).
    redirect "/animals/#{params[:id]}"
end

# GET requests to the delete path for an animal
get '/animals/:id/delete' do
    # Create a new connection to the 'database.sqlite3' using SQLite3 as our database adaptor, and store this connection in a variable called 'db'
    db = SQLite3::Database.new("database.sqlite3")
    # We want the results to be returned as hashes, rather than as (the default) arrays.
    db.results_as_hash = true
    # Call the execute method on the open database connection, passing in the SQL query to delete a record using data in the params hash
    db.execute("DELETE FROM animals WHERE id == #{params[:id]};")
    # Once a response has been received, close the connection to the database, since we can only have a limited number of concurrent database connections.
    db.close
    # We don't actually want to render a view here - there's no view that corresponds with a 'delete' - the delete action happens on the back end. Instead, once the code above has been run and the relevant record has been deleted in the animals table in the database, we want to redirect the browser to our GET '/animals' route (which displays the animal  index page).
    redirect "/animals"
end

# GET requests to the path for an animal
get '/animals/:id' do
    # Create a new connection to the 'database.sqlite3' using SQLite3 as our database adaptor, and store this connection in a variable called 'db'
    db = SQLite3::Database.new("database.sqlite3")
    # We want the results to be returned as hashes, rather than as (the default) arrays.
    db.results_as_hash = true
    # Call the 'execute' method on our database connection, passing in the SQL query we want to execute (one which will return a record from the animals table in the database, using the ID in the params hash (which will be whatever's in the :id space in the path /animals/:id/), and store the response in an instance variable called '@animal', which will make this variable available in the view rendered by this route (the animals view, below). We need to select the first record, since the response will be in an array.
    @animal = db.execute( "SELECT * FROM animals WHERE id == #{params[:id]};" ).first
    # Once a response has been received, close the connection to the database, since we can only have a limited number of concurrent database connections.
    db.close
    # Render the animals_show erb template (this will look for a file called animals_show.erb in our views folder)
    erb :animals_show
end
```

### **Homework**

* Build your own CRUD app! Use the Butterflies DB as your reference, but pick a topic of your own.

