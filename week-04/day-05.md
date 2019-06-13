---
description: >-
  Give a human a program, frustrate them for a day.Teach a human to program,
  frustrate them for a lifetime.
---

# Day 05

What we covered today:

* Object Oriented Programming
  * Objects
  * Classes
* Ruby - Variable Scope
* Ruby - Class and Instance Methods
* Active Record

### Warmup <a id="warmup"></a>

* [Point Mutations - Ruby​](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week04/day05_point_mutations)

### Classwork <a id="classwork"></a>

* ​[Codealong](https://github.com/wofockham/sei-31/tree/master/07-databases/butterflies_ar)​​

## Object Oriented Programming <a id="object-oriented-programming"></a>

### Brief Intro to OOP <a id="brief-intro-to-oop"></a>

Basically, OOP is an approach to development that tries to replicate real life. It is pretty much always done using objects or classes as namespaces\* and treats them as a way to make your code "modular".

\* Namespace: A namespace is an environment or container in which related entities are grouped, and within which a symbol or identified is unique.

The focus of object-oriented is on data and structure rather than logic. Small, clean methods are also a major part of it.

### Basic OOP in Ruby <a id="basic-oop-in-ruby"></a>

Up until now, we have only been modifying objects. We can create an empty object and then add methods and features to construct a particular object.

```ruby
o = {}
def o.silly_method    
    puts "I'm silly!"
end
```

This is not ideal. It means that every time we create an object, we have to add methods to that object - every single time, even if the methods are the same, and the objects in question are very similar.

To get around this we use classes!

### Classes <a id="classes"></a>

Everything in Ruby is an object, and every object is an "instance" of a class - it inherits from a class, and in some capacity will ultimately inherit from the Ruby `Object`. This doesn't pop up that regularly, but to see what I mean:

```ruby
{}.class
# => Hash
[].class
# => Array
"".class
# => String
​
​# etc.
```

If you want to see every class an object inherits from, we can call the `ancestors` method on the class.

```ruby
{}.class.ancestors
# => [Hash, Enumerable, Object, PP::ObjectMixin, Kernel, BasicObject]
[].class.ancestors
# => [Array, Enumerable, Object, PP::ObjectMixin, Kernel, BasicObject]
"".class.ancestors
# => [String, Comparable, Object, PP::ObjectMixin, Kernel, BasicObject]
```

Treat classes as a factory or a blueprint, something that gives another thing all the details that it needs to be created and used. We use them to stop duplicate code and to make manageable, easy-to-debug code.

#### _What do classes look like?_ <a id="what-do-classes-look-like"></a>

```ruby
class MarxBrother
end
```

#### _The real power of classes_ <a id="the-real-power-of-classes"></a>

The real power of classes comes from being able to add methods to _all_ objects of a given class, rather than to individual objects! We can encapsulate functionality with classes, which will be inherited by all objects that are an 'instance' of that class \(ie, an object created from that class blueprint\).

```ruby
class MarxBrother    
    def speak    
    end​    
    
    def laugh    
    end
end
```

Unlike JavaScript, though, we can't call the functions or methods on the class itself. Rather, we call methods on objects that are instances of the class. This is how we could achieve this in JavaScript:

```ruby
const MarxBrother = {    
    speak: function () {        
        console.log( "I can talk!" );    
    }
}
​
​MarxBrother.speak();
```

In Ruby, we need to create an instance of the class, and call the method on that instance. Think of the class as being a blueprint, and the instance object being the house that is built from the blueprint. You don't live in the blueprints for a house \(unless you're homeless and you've stumbled across some discarded architectural plans\) - you live in the house that's build from the blueprints.

```ruby
class MarxBrother    
    def speak        
    puts "I can talk!"    
    end​    
    
    def laugh    
    end
end​
​
groucho = MarxBrother.new
groucho.speak
# => "I can talk!"
```

Obviously, though, we want to be able to store something on the Marx Brother themselves! Maybe we want to know or set a Marx Brother's name, age or gender, for example.

We use what are called "getters" and "setters" to do this. These methods allow us to get attributes for particular objects, and to set attributes for particular objects

```ruby
class MarxBrother
    # This is a setter - it allows us to set the name for an object that inherits from
    # the MarxBrother class.
    def name=(name)
        # placing the '@' symbol before the variable name will give the variable Class
        # scope. This will be accessible from the other methods defined in the Class.
        @name = name
    end
​
    # This is a getter - it allows us to get, or read, the name for an object that 
    # inherits from the MarxBrother class.
    def name
        @name
    end
​
    def age=(age)
        @age = age
    end
​
    def age
        @age
    end
end
​
groucho = MarxBrother.new
groucho.name=("Groucho Julius Marx")
groucho.age=(80)
​
groucho.name
# => "Groucho Julius Mar"
groucho.age
# => 80
```

Well, fortunately there is - we can use the `attr_accessor` method to specify what attributes we want to be able to **get** and **set** for instances of a class.

```ruby
class MarxBrother
    attr_accessor :name, :age
end
​
groucho = MarxBrother.new
groucho.name = "Groucho Julius Marx"
groucho.name
# => "Groucho Julius Marx"
​
groucho.age = 100
groucho.age
# => 100
```

Okay, that's pretty cool, we no longer have to write out those getter and setter methods, but it would be _great_ if we could set those `name` and `age` attributes when we **instantiate** \(create\) a new instance of that class.

Luckily, we have a solution for this.

On the creation of a new instance in Ruby, when the .new method is called, it will also automatically call a method called `initialize`.

```ruby
class MarxBrother
​
    attr_accessor :name, :age
​
    # you can provide default values when defining the arguments
    def initialize( name, age=20 )
        @name = name
        @age  = age
    end
​
end
​
harpo = MarxBrother.new("Harpo", 47);
#<MarxBrother:0x007fc0ea8687d0 @age=47, @name="Harpo">
​
# if we don't specify the age argument it will take the pre-defined value.
harpo = MarxBrother.new("Harpo");
#<MarxBrother:0x007fc0ea8687d0 @age=20, @name="Harpo">
```

This will do everything that we were doing before.

**IMPORTANT**: The initialize method will only be called when we instantiate a new object if is has that exact spelling - `initialize`.

We can add heaps of methods and see what we have to work with as well.

```ruby
class MarxBrother
    def method_one
    end
    def method_two
    end
    def method_three
    end
end
​
groucho = MarxBrother.new
​
# To see all the methods we can call on instances of the class to which the 
#MarxBrother object belongs:
​
groucho.class.instance_methods
# This will return an enormous array of methods that the object inherits from Ruby.
​
# That's annoying. How can we just see the methods we've defined for that class?
​
# If you pass in the value false to the instance_methods method, it will only show you
# the methods you have declared in the class definition.
​
groucho.class.instance_methods( false )
# => :method_one, :method_two, :method_three
​
```



## Ruby - Variable scope <a id="ruby-variable-scope"></a>

There are four types of variable in Ruby, categorized by scope - ie, from where the variable is accessible within a program.

| Starts with | Variable scope | Example |
| :--- | :--- | :--- |
| `$` | Global | `$planet` |
| `@` | Instance | `@joel` |
| `@@` | Class | `@@habitat` |
| `[a-z]` or `_` | Local | `joel` |
| `[A-Z]` | Constant | `Pi` |

### Local variables <a id="local-variables"></a>

`animal = "badger"`

The scope of a local variable depends on where it was defined. The scope ranges from the class, module, method, or do block in which it is defined to the corresponding `end` \(from a block's opening brace to its closing curly bracket\).

### Instance variables <a id="instance-variables"></a>

`@animal = "badger"`

Instance variables are available throughout the instance of the class in which it was defined, and are limited to that specific instance object. If an instance variable is modified in one instance, it will not affect other objects of the same class - each instance has its own 'local' copy of the variable.

### Class variables <a id="class-variables"></a>

`@@habitat = "forest"`

Class variables are available throughout all instances of the class in which it was defined. These are often used to define characteristics that are common to all instances of a particular class, but should generally be avoided.

### Global variables <a id="global-variables"></a>

`$planet = "earth"`

Global variables are available throughout a program. Again, these should generally be avoided - not only are they visible throughout the program, they can also be _changed_ from anywhere in the program.

### Constants <a id="constants"></a>

`Pi = 3.1415926535`

The scope of a constant depends on where it was defined. Constants defined within a class or module can be accessed from within that class or module, and those defined outside a class or module can be accessed globally. Constants are useful for creating variables that you want to be available everywhere but don't don't _intend_ to be changed \(they _can_ be changed, they just aren't _intended_ to be changed\).

### Determining variable scope <a id="determining-variable-scope"></a>

While the scope of a variable can be deduced from the variable name, we can use the `defined?` method to programmatically determine the scope of a variable. The `defined?` method returns the scope of the variable passed into the method.

```ruby
animal = 'badger'
defined?(animal)
# => 'local-variable'

$instructor = 'Joel'
defined?($instructor)
# => 'global-variable'
```

## Ruby - Class and Instance Methods <a id="ruby-class-and-instance-methods"></a>

Methods in Ruby can broadly be categorised as `instance methods` and `class methods`

### Instance methods <a id="instance-methods"></a>

Instance methods are pieces of functionality that belong to a particular instance of a class. They cannot be called on the class itself, or on anything other than an instance of the class.

```ruby
class Animal

    def searching
        puts "searching for #{@animal}"
    end    
end

a = Animal.last
a.searching
# => "Searching for weasel"
```

### Class methods <a id="class-methods"></a>

Class methods are methods are pieces of functionality that belong to a class, but aren't tied to any particular instance of that class. There are a few ways to declare class methods, but the most common way is to prefix the method name with the word `self`.

```ruby
class Animal
    def self.searching
        puts "searching for animals"
    end    
end

Animal.searching
# => "searching for animals"
```

### Predicate methods <a id="predicate-methods"></a>

This isn't so much a different category of method as a type of method. A predicate method is any method that will only ever return `true` or `false`. The convention in Ruby is to end predicate methods in a `?` - the `?` isn't a magic symbol, here. It just reminds you and other developers that this method will return a boolean value, and makes your code read more like plain English.

```ruby
def is_big?
    @number > 10000
end

@number = 10001

puts "That's a big number" if is_big?
# => true
```

## Active Record <a id="active-record"></a>

### Preface - Object Relational Mapping <a id="preface-object-relational-mapping"></a>

One of the most significant principles in Object-Oriented Programming is the idea of rich objects - things that store data, and allow it to be retrieved in logical and concise ways. This pattern is what Active Record strives for. It's called Object Relational Mapping, or ORM. The basic principle of ORM is that rich objects in your application should be connected with database tables. Using ORM, the properties and relationships of the objects in an application can be easily stored and retrieved from a database without writing SQL statements directly and with less overall database access code, but instead by creating and manipulating Ruby objects. It is also far more secure as it reduces an application's susceptibility to SQL Injections.

### What does Active Record do? <a id="what-does-active-record-do"></a>

Active Record, as an ORM, gives us several mechanisms, with the most important being the ability to:

* Represent models and their data.
* Represent associations between these models.
* Represent inheritance hierarchies through related models.
* Validate models before they are persisted to the database.
* Perform database operations in an object-oriented fashion.

### Convention over configuration <a id="convention-over-configuration"></a>

The concept of convention over configuration is big in development generally, but it is particularly full on when it comes to Active Record. That is because it uses the names to sort out associations etc. Make sure you follow these rules!

* **Database Tables** - Plural with underscores separating words \(articles, line\_items etc.\)
* **Model / Class Names** - Singular with the first letter of each word capitalized \(Article, LineItem etc.\)

### How do you work with Active Record? <a id="how-do-you-work-with-active-record"></a>

Well, at the end of the day, Active Record is a Ruby gem like any other. So we need to require it!!

```ruby
require 'active_record' # make sure this is at the top of the file!
```

After requiring it, we also need to actually establish the connection to the database. These are annoying lines that Rails will eventually do for us, but for now we'll have to type them out.

```ruby
# Sets up our connection to the database.db we have created
ActiveRecord::Base.establish_connection(
  :adapter  => 'sqlite3',
  :database => 'database.sqlite3'
)

ActiveRecord::Base.logger = Logger.new(STDERR) # Logs out the Active Record generated SQL in the terminal. This isn't necessary but very helpful and cool to see what it is actually running
```

### CRUD, SQL, HTTP & Active Record - One Table to Rule Them All <a id="crud-sql-http-and-active-record-one-table-to-rule-them-all"></a>

| CRUD | HTTP Verb | SQL | Action | Active Record | URL | View |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| CREATE | POST \(or Put\) | INSERT | \#create | .create | /butterflies | N/A\*\* |
| READ | GET | SELECT | \#index | .find/.find\_by | /butterflies | index |
| ​ | ​ | ​ | \#show | .find/.find\_by | /butterflies/:id | show |
| ​ | ​ | ​ | \#new\* | .new \(-&gt; .save\) | /butterflies/new | new |
| ​ | ​ | ​ | \#edit\* | .find/.find\_by \(-&gt; .save\) | /butterflies/:id/edit | edit |
| UPDATE | POST \(or Patch\) | UPDATE | \#update | .update | /butterflies/:id | N/A\*\* |
| DELETE | POST/GET \(or Delete\)_\*_ | DELETE | \#delete | .destroy/.destroy\_all | /butterflies/:id | N/A |

\* It's important to remember that the `new` and `edit` actions do not correspond with `insert` and `update` database operations. These actions usually represent the new and edit forms for a record.

\*\* Similarly, it's important to remember that there are no views associated with the `create` and `update` and `delete` actions. These are post/put/delete requests to the web server, not get requests.

\*\*\* Delete requests are often sent using the POST or GET HTTP verbs, since the `delete` method is not yet supported by all browsers.

### CRUD with Active Record <a id="crud-with-active-record"></a>

Once we have our models defined \(i.e. our classes\), we can get into the CRUD stuff. There are always a lot of options of how to do this!

#### _Create_ <a id="create"></a>

```ruby
plant = Plant.new
plant.name = "Hibiscus"
plant.flowers = true
plan.save # YOU MUST RUN THIS LINE! (When using the 'new' approach)

# OR

Plant.create( :name => "Hibiscus", :flowers => true ) # Create runs the save automatically

# OR

user = User.new do |u|
  u.name = "David"
  u.occupation = "Code Artist"
end
```

#### _Read_ <a id="read"></a>

```ruby
Plant.all
Plant.first
Plant.last
Plant.find( 10 ) # Find with an ID
Plant.find_by( :name => "Hibiscus" ) # Returns the first plant that matches
Plant.where( :name => "Hibiscus" ) # Returns all instances where the plant matches
```

#### _Update_ <a id="update"></a>

```ruby
plant = Plant.find_by( :name => "Hibiscus" )
plant.name = "Hibiscus 2"
plant.save

plant = Plant.find_by( :name => 'Hibiscus 2' )
plant.update( :name => 'Hibiscus' ) # This will save automatically
```

#### _Delete_ <a id="delete"></a>

```ruby
plant = Plant.find_by( :name => 'Hibiscus' )
plant.destroy

Plant.destroy_all
```

### _Active Record - Recommended Readings_ <a id="active-record-recommended-readings"></a>

* ​[Rails Guides - Active Record Basics](http://guides.rubyonrails.org/v4.2/active_record_basics.html)​
* ​[Rails Guides - Active Record Query Interface](http://guides.rubyonrails.org/v4.2/active_record_querying.html)​

### Homework <a id="homework"></a>

* To create yet another sinatra CRUD application, this time with TWO tables associated in a one to many relationship. Good candidates: an author has many books, a director has many films, a musician has many songs, a sportsball team has many sportsball players
* Read:
  * ​[Treehouse Tutorials - Active Record Basics](https://teamtreehouse.com/library/activerecord-basics)​
  * ​[Rails Guides - Getting Started with Rails](http://guides.rubyonrails.org/v4.2/getting_started.html)​

