---
description: >-
  I always knew that one day Smalltalk would replace Java. I just didn't know it
  would be called Ruby - Kent Beck
---

# Day 02

What we covered today:

* Ruby - Collections
  * Arrays
  * Hashes
* Ruby Basics Part II

**Warmup**

* [Raindrops - Ruby](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week04/day02_ruby_raindrops)

## Ruby - Collections

### **Arrays**

### **Creating an array**

```ruby
# LITERAL CONSTRUCTOR

bros = []
bros = [ 'groucho', 'harpo', 'chico' ]

# CLASS KEYWORD

bros = Array.new # => []
bros = Array.new( 3 ) # => [ nil, nil, nil ]
bros = Array.new( 3, true ) # => [ true, true, true ]
# The parentheses are optional

bros = Array.new(4) { Hash.new } # => [{}, {}, {}, {}]
bros = Array.new(2) { Array.new(2) } # => [ [nil, nil], [nil, nil] ]
# The curly brackets are optional

# A FEW SHORTCUTS FOR CREATING BASIC ARRAYS

bros = 'groucho chico harpo'.split(' ') # => ['groucho', 'harpo', 'chico']
bros = %w(groucho harpo chico) # => ['groucho', 'harpo', 'chico']
# you can also use curly, square, or any character instead of the parentheses
# bros = %w !groucho, harpo, chico! => ['groucho', 'harpo', 'chico']

# CHARACTER MODIFIERS - http://en.wikibooks.org/wiki/Ruby_Programming/Syntax/Literals

%w{ Hello World }
%w[ Hello World ]
%w/ Hello World /
```

### **Accessing elements of an array**

```ruby
arr = [1, 2, 3, 4, 5, 6]
arr[2]      # => 3
arr[100]    # => nil
arr[-1]     # => 6
arr[-3]     # => 4

arr[2, 3]   # => [3, 4, 5]
# Adding an additional argument will return the more result depending on the number given.

arr[1..4]   # => [2, 3, 4, 5]
arr[-1..-2] # => [5, 6]

# These work for reassignment as well!

arr[0] = 0
arr[0] = 1

arr.at(0)   # => 1

arr.first   # => 1
arr.last    # => 6

arr.take(3) # => [1, 2, 3] - Grabs the first three elements
arr.drop(3) # => [4, 5, 6] - Grabs the last three elements

arr.fetch(100) # => IndexError: index 100 outside of array bounds: -6...6
arr.fetch(100, "ERROR") # => "ERROR"
```

### **Adding items to an array**

```ruby
arr = [1, 2, 3, 4]
arr.push(5) # => [1, 2, 3, 4, 5]
arr << 6    # => [1, 2, 3, 4, 5, 6] Uses push behind the scenes

arr.unshift(9) # => [0, 1, 2, 3, 4, 5, 6] Adds an element to the start

arr.insert( 3, 'Serge' ) # => [ 0, 1, 2, 'Serge', 3, 4, 5, 6 ]
arr.insert( 4, 'didnt marry', 'Jane') # =>  [0, 1, 2, 'Serge', 'didnt marry', 'Jane', 3, 4, 5, 6]
```

### **Removing items from an array**

```ruby
# Pop removes the last element and returns it (it is destructive)

arr = [1, 2, 3, 4, 5, 6]
arr.pop     # => 6
arr         # => [1, 2, 3, 4, 5]

# To retrieve and at the same time remove the first item

arr.shift # => 1

# Delete at a particular index

arr.delete_at( 2 )

# To delete a particular element anywhere

arr = [1, 2, 2, 3]
arr.delete(2) # => [1, 3]

# Compact will remove nil values

arr = ['foo', 0, nil, 'bar', 7, 'baz', nil]
arr.compact  #=> ['foo', 0, 'bar', 7, 'baz']

# Remove duplicates

arr = [2, 5, 6, 556, 6, 6, 8, 9, 0, 123, 556]
arr.uniq    # => [2, 5, 6, 556, 8, 9, 0, 123]
```

### **Iterating over arrays**

```ruby
arr = [1, 2, 3, 4, 5]

arr.each do |el|
    puts el
end

arr.each { |el| puts el }

arr.reverse_each do |el|
    puts el
end

arr.reverse_each { |el| puts el }

# The map method will create a new array based on the original one, but with the values modified by the supplied block

arr = [1, 2, 3]
arr.map { |a| 2 * a }  # => Returns [ 2, 4, 6 ] but doesn't change the original
arr.map! { |a| 2 * a } # => Changes the original and returns it

# DON'T DO IT THESE WAYS!

arr = [1,2,3,4,5,6]
for x in 0..(arr.length-1)
    puts arr[x]
end

# or, with while:
x = 0
while x < arr.length
    puts arr[x]
    x += 1
end

for el in arr
    puts el
end
```

### **Selecting items from an array**

Elements can be selected from an array according to criteria defined in a block. The selection can happen in a destructive or a non-destructive manner. While the destructive operations will modify the array they were called on, the non-destructive methods usually return a new array with the selected elements, but leave the original array unchanged.

```ruby
arr = [1, 2, 3, 4, 5, 6]
arr.select { |a| a > 3 }        # => [4, 5, 6]
arr.reject { |a| a < 4 }        # => [4, 5, 6]

# You can use these two with the exclamation mark to make them destructive

# The next two are destructive!

arr.delete_if { |a| a < 4 }     # => [4, 5, 6]
arr.keep_if { |a| a < 4 }       # => [1, 2, 3]
```

### **Ruby array comparison tricks**

```ruby
array1 = ["x", "y", "z"]
array2 = ["w", "x", "y"]

array1 | array2
# Combine Arrays & Remove Duplicates(Union)
# => ["x", "y", "z", "w"]

array1 & array2
# Get Common Elements between Two Arrays(Intersection)
# => ["x", "y"]

array1 - array2
# Remove Any Elements from Array 1 that are contained in Array 2.(Difference)
# => ["z"]
```

Check out [this site](https://sites.google.com/site/dhtopics/Home/ruby-essentials/advanced-ruby-arrays)

**Ruby - Collections - Arrays - Exercises**

* [Arrays Exercises](https://gist.github.com/wofockham/64bc0aa2857797dc4e57)
* [Solution](https://github.com/wofockham/wdi-30/blob/master/05-ruby/arrays.rb)

## Ruby Fundamentals - Part II

### **Destructive methods vs non-destructive methods**

There are destructive methods and non-destructive methods in Ruby. Destructive methods will affect the original, whereas non-destructive will leave it alone and just return an altered copy. Destructive methods normally end with an !.

### **Predicate methods**

More or less, predicate methods are those that return a boolean value. They always end with a ?.

### **Blocks**

The content in iterators are called blocks, they are quite similar to anonymous functions in javascript.

```ruby
arr = [1, 2, 3]

# The content between the do and the end is the block
arr.each do |el|
    puts el
end

# The content between the curly brackets is the block
arr.each { |el| puts el }
```

### **Object IDs, strings vs. symbols, false interlude**

In Ruby, every single thing is an object. Absolutely everything. Doesn't matter if it is a string, boolean or anything - they are all objects and all get assigned an object\_id. Every time a new one is created, even if it looks identical, a new object\_id \(a new place in memory\) will be created.

```ruby
"Joel".object_id
# => 70131971988560

{}.object_id
70131953807740

false.object_id
0
```

Each time you reference a new string, hash or array, it declares a new object\_id - meaning that it takes up more memory in your Ruby program. If you imagine a database with thousands of entries, these small things add up to huge amounts of memory.

Symbols aren't like that, they are assigned a static place in memory \(meaning that they don't need to be redefined\). They behave in the exact same way as strings are easily translated back. Always use symbols for keys on objects.

```ruby
:joel.object_id
1147228
```

```ruby
# Conversions!

:joel.to_s
# => "joel"

"joel".to_sym
# => :joel
```

### **False interlude**

The only things that are considered false in Ruby are the boolean `false`, and the `nil` value.

## Collections

### **Hashes**

### **Creation of a hash**

```ruby
# Literal Constructor
# "=>"" is called a hash rocket

hash = {}
serge = {
    :name => "Serge",
    :nationality => "French"
}
serge = {
    "name" => "Serge",
    "nationality" => "French"
}
serge = { # Keys stored as symbols!
    name: "Serge",
    nationality: "French"
}
# This will be automatically converted to 'hash rocket' styles

# Class Constructor

hash = Hash.new

# Normally a hash will return nil if the property is undefined
# We can pass in default values to this quite easily though

hash = Hash.new( "JOEL" )
hash["JOSH"] #=> Will return "JOEL"

# If you create the hash using the literal though...

hash = {}
hash.default = "JOEL"
hash["JOSH"] #=> Will return "JOEL"
```

### **Accessing elements**

```ruby
serge = { # Keys stored as symbols!
    name: "Serge",
    nationality: "French"
}


serge[:name]

serge = {
    "name" => "Serge",
    "nationality" => "French"
}

serge["name"]
```

### **Adding items to a hash**

```ruby
# Notice no hash rocket!

serge[:counterpart] = "Jane (temporarily)"

# This is the same way as you access them!

p serge[:counterpart] # => "Jane (temporarily)"
```

### **Removing items from a hash**

```ruby
serge.delete(:counterpart)
```

### **Iterating over hashes**

```ruby
serge = { # Keys stored as symbols!
    name: "Serge",
    nationality: "French"
}

# Will run for keys and values
serge.each do |all|
    puts all
end

# Will run for each key and value pair
serge.each do |key, value|
    puts "Key: #{key} and Value: #{value}"
end

# Return the current key
serge.keys.each do |key|
    puts key
end

# Return the current value
serge.values.each do |value|
    puts value
end

# Thousands of other ways to do this though
```

### **Ruby - Collections - Exercises**

* [Array and hash access](https://gist.github.com/wofockham/50a52e9399075709fe87)
* [Solution](https://github.com/wofockham/wdi-30/tree/master/05-ruby)

### Homework

* [MTA II - Ruby](https://gist.github.com/wofockham/399e315a90e04a867455)



