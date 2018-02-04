---
title: Ruby Notes
tags: Ruby
---

# General
Everything in Ruby is an object.

`nil` is the null in Ruby.

Use `require` to include other files in workspace.

`irb` is the interactive ruby shell.

## Comment
`#` forms a single-line comment.
```ruby
=begin
...
=end
```
is multiline comment, but no one uses them.

## Function not found
If a function (or a method) is not found, Ruby calls the `method_missing` method with the function name as `id` and parameters as `*arguments`. By default `method_missing` raises a NameError exception.

# Operations
`**` for power.

Arithmetic is actually method on an object: `1+3` is actually `1.+(3)`.

## Boolean
* Equality: `==`
* Inequality: `!=`
* Negation: `!`
* And: `&&`
* Or: `||`

`and` and `or` keywords are like `&&` and `||` in bash. They are only used as flow-control and never use them in boolean operation:

* `do_something() and do_something_else()` call the latter only the former succeeds.
* `do_something() or do_something_else()` call the latter only the former fails.


In Ruby, only `nil` and `false` are falsey, 0 is evaluated to `true`.

# Data Structure
Assignment returns right-hand-side value, so assignments can be chained:

```ruby
x = y = 10
```

Variable names uses snake_case (lower-case words chained with underscore), just like in C++ and Python.

## Strings
Prefer single quoted strings for string literals, and use double quoted strings for additional inner calculations such as substitution: `#{val}` inside another string invokes substitution of `val` into the place.

`+` concatenate strings, but not with numbers. Call `to_s` method to convert number to strings.

`puts` is Ruby's printing method. It returns `nil`.

## Symbols
`:pending` is a symbol. It is immutable, reusable constants represented by an interger value. It favors over strings for comparison.

Symbol is about **who** it is, not **what** it is. All references to a symbol shares the same object ID (hash key), but two strings with same content may not.

## Arrays
Comma-separated values enclosed in square brackets. Values in an array can have different types.

Array index is zero-based, and accessor `[]` is also a method. 

* `array[-1]` points to the last element.
* `array[2, 3]` returns 3 elements from index 2 in a new array.
* `array[1..3]` returns 3 elemens from index 1 to 3 in a new array.
* `array << val` appends `val` to the end of `array`.
* `array.include?(1)` checks if 1 is in `array`.

## Hash
Hash is Ruby's primary dictionary with key/value pairs.
```ruby
hash = { 'color' => 'green', 'number' => 5 }
new_hash = { defcon: 3, action: true }
```
	
Second form uses symbols as keys, and supported from Ruby 1.9.

* Look up by key: `hash['color']`. If the key doesn't exist, returns `nil`.
* Key set: `hash.keys`, value set: `hash.values`.
* Check existence: `hash.has_key?('color')`, `hash.has_value?(5)`.

# Conditions
## if
```ruby
if true
	'if statement'
elsif false
	'else if, optional'
else
	'else, optional'
end
```
Do not use parentheses around the condition of if.

## Ternary if
`?:` exists in Ruby. However, only very simple conditional are recommended to use this form.

## Unless
`unless` is the opposite of `if`. If the value is **false** then the expression is executed. Don't use `unless` with `else`, use `if` instead.

## Modifier if and unless
You can use `if` and `unless` and modifiers:
```ruby
<expr> if <condition>
```

Then `<expr>` is executed only when `<condition>` evaluated to true.

## case
```ruby
case grade
when 'A'
	puts 'Way to go kiddo'
when 'B'
	puts 'Better luck next time'
when 'C'
	puts 'You can do better'
when 'D'
	puts 'Scraping through'
when 'F'
	puts 'You failed!'
else
 	puts 'Alternative grading system, eh?'
end
```

Cases can also use ranges:
```ruby
case grade
when 90..100
 		puts 'Hooray!'
when 80...90
 		puts 'OK job'
else
 		puts 'You failed!'
end
```

# Loops
## for loops
```ruby
for counter in 1..5
	puts "#{counter}"
end
```

**But no one uses for loops**. Instead passes a block to `each` method. A block is like lambdas, anonymous functions.
```ruby
(1..5).each do |counter|
	puts "#{counter}"
end
```

Or: `(1..5).each { |counter| puts "#{counter}" }`.

Arrays and hashes can be iterated using `each`.

## while loops
```ruby
counter = 1
while counter <= 5
	puts "iteration #{counter}"
	counter += 1
end
```

After the while condition, there is an optional `do`, but it is omitted.

## until loop
until loop executes while the condition is **false**.

## Modifier while and until
Like `if` and `unless`, `while` and `until` can be used as modifiers.

## Infinite loop
```ruby
loop do
	# break somewhere
end
```

creates an infinite loop. It's generally the same as `while true`.

## Loop controller
* `break`: leave the loop block early.
* `next`: skip the rest of the current iteration.
* `redo`: redo the current iteration.

# Exception handling
```ruby
begin
	# code here that might raise an exception
	raise NoMemoryError, 'You ran out of memory.'
rescue NoMemoryError => exception_variable
 	puts 'NoMemoryError was raised', exception_variable
rescue RuntimeError => other_exception_variable
 	puts 'RuntimeError was raised now'
else
 	puts 'This runs if no exceptions were thrown at all'
ensure
 	puts 'This code always runs no matter what'
end
```

# Functions
```ruby
def sum(x, y)
 	x + y
end
```
Use parentheses when there are arguments, and omit parentheses when the method doesn't accept any arguments.

Functions and blocks returns the value of the last statement.

Parentheses are optional when calling methods. Parameters are separated by commas.
```ruby
sum 3, 4
sum sum(3, 4), 5 # resolve ambiguity
```
To use a block as parameter, use `&`:
```ruby
def guests(&block)
 		block.call 'some_argument'
end
```

To pass arbitrary number of parameters, use `*`. These parameters will be converted into an array:
```ruby
def guests(*array)
 		array.each { |guest| puts guest }
end
```

## Yield
All methods have an implicit, optional block parameter which can be called with the `yield` keyword. The `yield` keyword calls the code supplied in the block.

```ruby
def surround
	puts "{"
		yield
	puts "}"
end

surround { puts 'hello world' }
```
Function call to surround replaces the block into the `yield` keyword.

# Classes
* Definition:
```ruby
class Human
	# something
end
```
* Class variables, shared by all instances of this class and all its descendants: `@@species = 'Human'`
* Constructor is a method called `initialize`. Instance variables are defined here using `@name`.
* Getters and setters:
```ruby
# Basic setter method
def name=(name)
	@name = name
end

# Basic getter method
def name
	@name
end
```
 However, getters and setters can be generated using `attr_accessor :name`, or individually using `attr_reader :name` and `attr_writer :name`. Variables after `attr_accessor` keyword can be chained together using commas.
  
* Class methods: use `self` keywords like `def self.say(msg)`.
* Instantiate a class object: `jim = Human.new('Jim')`.

## Singleton classes
The singleton class of an object is a class that holds methods for only that instance. To define it, using `class << <object>`.

Often, singleton class are defined like this:

	class C
		class << self
			# ...
		end
	end

This allows definition of class methods without the `self` keywords, and they are grouped into the singleton class for better readability.


## Variable scope
* Global: `$var`
* Instance: `@var`
* Class: `@@var`
* Constant: `Var`

Note `var` and `$var` is two different variables.

## Accessibility
By default, methods are public. Access modifier (`private`, `protected`, `public`) continues until the end of the scope, or another access modifier pops up. So a modifier will last to the end of the class if no other modifiers pop up.

A protected method can be called from a class or descendant class instances, but also with another instance as its receiver.

A private method cannot be called outside the instance.

## Modifiability
Ruby class are open. You can redefine the class and adding, changing or removing things of that class.

To open a class, simply redefine it as normal class definition.
* Adding: simply define it.
* Changing: use `alias <new_name> <old_name>` to change name.
* Removing a method: use `remove_method :<my_method>`.


## Inheritance
```ruby
class Children < Parent
end
```
Note class variables are shared between base class and all child classes; but instance variable is not shared.

To call the overloaded method in parent class from child class, use `super` keyword.

```ruby
class Foo
  def foo
    "#{self.class}#foo"
  end
end

class Bar < Foo
  def foo
    "Super says: #{super}"
  end
end
```

## Method naming conventions
* Methods that answer questions (returns `true` or `false`) end in question marks.
* Methods that are potentially dangerous end with exclamation marks.

# Module
A module is a collection of methods and constants.
```ruby
module ModuleExample
	def foo
		'foo'
	end
end
```
To use the module, use `include` or `extend` keyword:
```ruby
class Person
	include ModuleExample
end

class Book
	extend ModuleExample
end
```
* `include` modules binds the methods to objects of the class.
* `extend` modules binds the methods to the class.

Modules can be nested. You can physically put inner module inside the outer module, or use `::` like `.` in Java provided the outer modules are already defined.

# Embedded Ruby
Ruby provides a program called ERB (Embedded Ruby). It allows you to put Ruby codes inside an HTML file. ERB reads along, word for word, and then at a certain point, when it encounters a Ruby code embedded in the document, it starts executing the Ruby code.

The syntax is similar as JSP.

You need to know only two things to prepare an ERB document:
* If you want some Ruby code executed, enclose it between `<%` and `%>`.
* If you want the result of the code execution to be printed out, as a part of the output, enclose the code between `<%=` and `%>`.

```erb
<% page_title = "Demonstration of ERB" %>
<% salutation = "Dear programmer," %>

<html>

   <head>
      <title><%= page_title %></title>
   </head>
	
   <body>
      <p><%= salutation %></p>
      <p>This is an example of how ERB fills out a template.</p>
   </body>
	
</html>
```