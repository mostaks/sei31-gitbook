---
description: 'Ruby, Ruby, Ruby, Ruby. Due to lack of interest Tomorrow is cancelled.'
---

# Day 01

What we covered today:

* Ruby - Installation and RVM
* Ruby - A Brief History of Ruby
* Ruby - Introduction
  * Data Types
    * Strings and Numbers
  * Operators
  * Variables
  * Methods
  * Ruby - Fundamentals - Part I
  * Conditionals
  * Control Structures
* Ruby - Methods

**Warmup**

* [Text Soup: Trump Edition](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week04/day01_text_soup)

## Ruby - Installation and RVM <a id="ruby-installation-and-rvm"></a>

* ​[RVM and Ruby Installation Guide](https://gist.github.com/ga-wolf/98718aa5c63d8323ae46)​

### How to get Ruby <a id="how-to-get-ruby"></a>

Install Developer Tools from Xcode \(this should have happened for most of you\):

`xcode-select --install`

Install HomeBrew:

`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

Access a specific URL using a secured line and run the downloaded program:

`curl -sSL https://get.rvm.io | bash -s stable`

Restart the terminal, and try running the command:

`rvm`.

If it doesn't work...

* Open the bash\_profile up in Atom

  `atom ~/.bash_profile`

* Add these lines into the bottom of the .bash\_profile and save it

  `[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm"`

  `export PATH="$PATH:$HOME/.rvm/bin"`

Restart the terminal and run the following commands to make sure it's all cool:

`rvm`

`rvm list known`

`rvm get stable --auto`

Go here and find the most recent version - [https://www.ruby-lang.org/en/downloads/](https://www.ruby-lang.org/en/downloads/)​

Latest version at the time of writing was 2.4.0, swap those in for the latest stable version that shows on that website

`rvm install ruby-2.4.0`

`rvm --default use 2.4.0`

Let's test that it has all worked.

* `ruby -v`
* `rvm -v`
* `which ruby` - should not return anything in the /usr/local/bin
* `which rvm`

If all of this has worked, run...

`gem install pry`

### Common Commands <a id="common-commands"></a>

`ruby -v` - Will return the current version of Ruby

`which ruby` - What is the path to the version of Ruby you are using

`ruby hello_world.rb` - Runs the hello\_world.rb file

`irb` - Runs a ruby console

`pry` - Runs a better console

`<CTRL> + D` - Ends a file running in irb or ruby

## Ruby - A Brief History of Ruby <a id="ruby-a-brief-history-of-ruby"></a>

Created in 1993 by Yukihiro Matsumoto \(Matz\). He knew Perl and he also knew Python. He thought that Perl smelt like a toy language apparently, and he disliked Python because it wasn't a true object-oriented programming language. Ruby was primarily influenced by Perl and SmallTalk though.

Matz wanted a language that:

* Syntactically Simple
* Truly Object-Oriented
* Had Iterators and Closures
* Exception Handling
* Garbage Collection
* Portable

### Programming Mottos <a id="programming-mottos"></a>

* Perl - There's More Than One Way To Do It \(T.M.T.O.W.T.D.I\)
* Python - There Should Be One And Only One Way To Do It
* Ruby - Designed For Programmer Happiness

### _Further Reading - A Brief History of Ruby_ <a id="further-reading-a-brief-history-of-ruby"></a>

* ​[SitePoint - History of Ruby](http://www.sitepoint.com/history-ruby/)​

## Ruby - Introduction <a id="ruby-introduction"></a>

### Data Types - Strings and Numbers <a id="data-types-strings-and-numbers"></a>

#### _Strings_ <a id="strings"></a>

Again, delimited by quotes.

`"string"` `'string'`

#### _Numbers_ <a id="numbers"></a>

There are multiple types:

* FixNum
* Float
* BigNum

### Operators <a id="operators"></a>

#### _Arithmetic Operators_ <a id="arithmetic-operators"></a>

Lots of them, but the basic ones are:

```ruby
+     # Addition
-     # Subtraction
*     # Multiplication
/     # Division
**    # Exponent (to the power of)
%     # Modulus
```

#### _Assignment Operators_ <a id="assignment-operators"></a>

```ruby
=     # Assignment
+=    # Add then assign
-=    # Subtract then assign
*=    # Multiply then assign
/=    # Divide then assign
%=    # Modulus then assign - assigns the remainder of the modules to the left 
      #operand
```

#### _Logical Operators_ <a id="logical-operators"></a>

```ruby
&&    # And
and   # And (alternative to &&)
||    # Or
or    # Or (alternative to ||)
!     # Not
not   # Not (alternative to !)
```

#### _Comparison Operators_ <a id="comparison-operators"></a>

All the usual suspects, plus a few new ones:

```ruby
>       # Greater than
>=      # Greater than or equal to
<       # Less than
<=      # Less than or equal to
==      # Equality (generally just use the double equals in Ruby)
!=      # Inequality
<=>     # Combined comparison - returns -1 if less than, 0 if equal, and 1 if greater 
        #than
.eql?   # True if the receiver and argument have the same type and equal values
.equal? # True if the receiver and the argument have the same object ID
```

### _Ruby - Operators - Recommended Readings_ <a id="ruby-operators-recommended-readings"></a>

* ​[Tutorials Point - Ruby Operators](https://www.tutorialspoint.com/ruby/ruby_operators.htm)​

### Variables <a id="variables"></a>

Unlike JavaScript, we don't need to use the `let` or `const` \(or any other\) keyword when declaring a variable in Ruby.

`ruby = "is nice"`

It's much harder to accidentally make global variables in Ruby.

`$ruby = "is nice"`

To make a `const` variable you must define the variable name ALL in caps.

`RUBY = "is nice"`

Ruby will allow you to change this constant variable but will notify you if you change it.

### Methods \(Functions in JS\) <a id="methods-functions-in-js"></a>

`puts "this is like console.log`

`print "this is also like console.log`

 `p "this is a bit more complex`

Parentheses in method calls are mostly optional in Ruby, but they are occasionally necessary \(eg, in method or function chaining\)

```ruby
puts("this")
puts "is the same as this"
```

Methods in Ruby have an **implicit return**, meaning that you don't need to use the `return` keyword in your methods - Ruby returns the last statement in a method on its own.

## Ruby Fundamentals - Pt I <a id="ruby-fundamentals-pt-i"></a>

#### _Basic naming conventions_ <a id="basic-naming-conventions"></a>

snake\_case\_everywhere - it's very rare to see camelCase!

#### _Variable interpolation in strings_ <a id="variable-interpolation-in-strings"></a>

String interpolation is where we put Ruby code to be evaluated inside a string - the code to be evaluated is inserted within `#{}`. This is most often used when interpolating variables, but we can put any expression within the curly brackets \(eg `#{5 + 4}` will interpolate `9` into the string\).

```ruby
name  = "gilberto"
drink = "scotch"

"My name is #{ name } and I drink #{ drink }!"
# A lot nicer than "my name is " + name + " and I drink " + drink
# Which is the way you would do it in JS
```

IMPORTANT: Interpolation only works with double quotes!! Single quotes mean 'leave this string alone, this is mine'.

#### _Comments in Ruby_ <a id="comments-in-ruby"></a>

```ruby
# This is is a single line comment

# This is
# a multiline
# comment

# OR (don't do this)

=begin
This is also a multi line comment
You can't have any an empty line between the =begin and the start of the comment
=end
```

#### _Getting user input_ <a id="getting-user-input"></a>

In JavaScript, we have `alert` and `prompt`, in Ruby we have `puts` and `gets`.

```ruby
# Initial greeting
puts "What is your first name?"

# first_name = gets
# This will wait for user input, and include the new line in the variable

first_name = gets.chomp
# This will wait for user input, and strip the new line from the variable
# For more documentation on chomp - http://ruby-doc.org/core-2.2.0/String.html#method-i-chomp

puts "Your first name is #{ first_name }."

p "What is your surname? " # using `p` will allow you to put the input on the same line.
surname = gets.chomp # Chaining
puts "Your surname is #{ surname }."

puts "Your full name is #{ first_name } #{ surname }"
# fullname = "#{ first_name } #{ surname }"
# Sames as ... puts "Your full name is #{ fullname }"

puts "What is your address?"
address = gets.chomp
puts "Your name is #{ fullname } and you live at #{ address }"

# INTERPOLATION ONLY WORKS IN DOUBLE QUOTES!
```

### Conditionals <a id="conditionals"></a>

#### `If` _statements_ <a id="if-statements"></a>

```ruby
if 13 > 10
    p "Yep, it is a bigger number"
end

grade = "A"

if grade == 'A'
    puts "You are killing it"
elsif grade == 'B'
    puts "You are coasting fine"
elsif grade == 'C'
    puts "Acceptable"
else
    puts "Please see me after class"
end

p "Yep, it is a bigger number" if 13 > 10 # This only works in single line statements

# It's called a modifier (if modifier)
```

#### `Unless` _statements_ <a id="unless-statements"></a>

```ruby
x = 1
unless x > 2
    puts "x is less than 2"
else
    puts "x is greater than 2"
end

# Modifier or backwards form
code_to_perform unless conditional
```

#### `case` _statements_ <a id="case-statements"></a>

Think of these as shorter if statements, but don't overuse them \(particularly in JS\)

```ruby
grade = 'B'
case grade
when 'A'
    p 'You are killing it'
when 'B'
    p 'You are coasting fine'
when 'C'
    p 'Acceptable'
else
    p 'Please see me after class'
end

case expression_one
when expression_two, expression_three
    statement_one
when expression_four, expression_five
    statement_two
else
    statement_three
end

# Very similar to the switch statement in JavaScript!
```

The case statement will compare the **case expression** with whatever a particular **when expression** evaluates to. This is fine when testing direct equality between the case expression and the when expressions. Example:

```ruby
score = 1
case score
when 1
  print "You got one"
when 2
  print "You got two"
end
# => "You got one"
```

The first `when` expression - `1` - evaluates to `1`, and since `1 == 1` evaluates to `true`, the first `when` expression is satisfied, and "You got one" is printed

But this is a problem when our `when` expression contain is more complex. Consider the following:

```ruby
score = 1
case score
when score <= 1
  print "You got one or less"
when score > 1
  print "You got two or more"
end
# => nil
```

The first `when` expression - `score <= 1` - evaluates to `true`, and since `1 != true`, so none of the `when` expressions will be satisfied.

For complex expressions, the case expression should be left blank.

```ruby
score = 1
case
when score <= 1
  print "You got one or less"
when score > 1
  print "You got two or more"
end
# => "You got two or more"
```

### _Ruby - Conditionals - Exercises_ <a id="ruby-conditionals-exercises"></a>

* ​[Drinking Age, Air Conditioning, Sydney Suburbs​​](https://gist.github.com/wofockham/67b291148a9efb6a7138)
* [​ Solution​​](https://github.com/wofockham/wdi-30/tree/master/05-ruby/conditionals)

### Control Structures <a id="control-structures"></a>

#### _While Loops_ <a id="while-loops"></a>

```ruby
while conditonal
    statement
end

while true
    p "OMG"
end # BAD IDEA

i = 0
while i < 5
    puts "i: #{ i }"
    i += 1
end
```

#### _Until Loops_ <a id="until-loops"></a>

```ruby
until conditional
    statement
end

i = 0
until i == 5
    puts "i: #{ i }"
    i += 1
end
```

#### _Iterators_ <a id="iterators"></a>

So, so common in Ruby.

```ruby
5.times do
    puts "OMG"
end
# => "OMG
# => "OMG
# => "OMG
# => "OMG
# => "OMG

5.times do |i|
    puts "i: #{ i }"
end
# => I: 0
# => I: 1
# => I: 2
# => I: 3
# => I: 4
# => I: 5


5.downto(0) do |i|
    puts "i: #{ i }"
end
# => I: 5
# => I: 4
# => I: 3
# => I: 2
# => I: 1
# => I: 0
```

#### `for` _loops_ <a id="for-loops"></a>

**For loops** are very rarely used in Ruby.

```ruby
# Don't ever use them
for i in 0..5
   puts "i: #{ i }"
end
# => I: 0
# => I: 1
# => I: 2
# => I: 3
# => I: 4
# => I: 5
```

#### _Generating random numbers_ <a id="generating-random-numbers"></a>

```ruby
Random.rand # Generates a number between 0 and 1
Random.rand(10) # Generates a random number up to 10 (including zero and 10)
Random.rand(5..10) # Generates a number between 5 and 10 (also includes them)
Random.rand(5...10) # Does not include 5 and 10
```

### _Ruby - Control Structures - Exercise_ <a id="ruby-control-structures-exercise"></a>

* ​​[Guess The Number​​](https://gist.github.com/wofockham/275f43b104c81c641849)
* ​[Solution​​](https://github.com/wofockham/wdi-30/tree/master/05-ruby)

## Methods <a id="methods"></a>

Methods are declared using the `def` keyword and called using the method's name.

### Methods with no arguments <a id="methods-with-no-arguments"></a>

```ruby
# Method definition
def hello
    puts "Hello"
end

# Method call
hello
# => "Hello"
```

### Methods with arguments <a id="methods-with-arguments"></a>

For methods with defined parameters, arguments can be passed in with or without parentheses.

```ruby
def hello( name )
    puts "Hello, #{name}"
end

hello("Josh")
# => "Hello, Josh"

hello "Josh"
# => "Hello, Josh"
```

If a method is defined with one or more parameters but is called without passing in the right number of arguments, an error will be thrown \(`Argument Error: wrong number of arguments`\).

### Methods with default arguments <a id="methods-with-default-arguments"></a>

To avoid `Argument Error`s thrown by calling an argument without the requisite number of arguments, we can set default parameters in the method definition.

```ruby
def hello( name = "World" )
  puts "Hello, #{name}"
end

hello
# => "Hello, World"

hello("Josh")
# => "Hello, Josh"

3.times { hello("Josh") }
# => "Hello, Josh"
# => "Hello, Josh"
# => "Hello, Josh"

def add(a, b)
  return a + b
end

add 4, 11
# => 15

new_total = add 4, 11
new_total
# => 15
```

### _Ruby \(General\) - Recommended Readings_ <a id="ruby-general-recommended-readings"></a>

* ​[Why's \(Poignant\) Guide To Ruby](http://poignant.guide/)​
* ​[The Bastard's Book of Ruby](http://ruby.bastardsbook.com/)​

## Homework <a id="homework"></a>

* ​​[Calculator​​](https://gist.github.com/wofockham/2752aa06121df7f3024c)
* Also:
  * Watch [Pry - Introductory Screencast](http://pryrepl.org/)​
  * Browse [Ruby - Documentation](https://www.ruby-lang.org/en/documentation/)​

