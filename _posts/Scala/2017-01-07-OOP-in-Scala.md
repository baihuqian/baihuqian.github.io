---
title: Learning Scala - Object-Oriented Programming in Scala
tags:
 - Scala
---

This article outlines classes, objects, and traits, and Object-Oriented Programming in Scala.

* TOC
{:toc}

# Class
In Scala, a class is not declared as public. A Scala file can contain multiple classes, and all of them have public visibility.

## Fields
There are two ways to define fields for a class: after class name or in the class body:

```scala
class Counter(private var value: Int) {
	def increment() {value += 1}
	def current() = value
}
```
This produces a constructor with one parameter.

```scala
class Counter {
	private var value = 0 // must initialize the field
	def increment() {value += 1}
	def current() = value
}
```
This produces a constructor with 0 parameters.

Fields are private in Java's sense. Their visibility is controlled by their accessors and mutators.


### `this.type` field
There is a special field in every object in scala: `this.type`, which is the actual type of the object. It is very useful as return types to support method chaining and inheritance. Such methods always declare `this.type` as return type and `this` as return value.

```scala
def setInt(i: Int): this.type = {
    this.i = i
    this
}
```

### Accessors and Mutators
Java's getters and setters are named after Java Bean convention. In Scala, they are named accessors and mutators. Scala will generate accessors and mutators to Java-type getter and setter with `@BeanProperty` quantifier.

The visibility of accessors and mutators depends on the type of fields and their visibility:
* `var` field: its accessor and mutator are public. Its mutator is named as `p_=`. When `obj.p = q` is called, the mutator method is executed.
* `val` field: only accessor is generated.
* A field without a `var` or `val` modifier: no accessor or mutator is generated.
* `private val` or `private var`: no accessor or mutator is generated.

Directly overriding generated accessors and mutators will result in compile-time error, as compiler confuses the field and method with the same name. However, one can mark the variable as private (to prevent compiler from generating accessors and mutators) with a different name (often with a leading underscore):

```scala
class Person(private var _name: String) {
    def name = _name
    def name_=(aName: String) { _name = name }
}
```

### Methods
Methods are public by default. Methods are a special type of functions.

### Object equality
Like Java, `equals` method is defined to compare two objects of the same type; but unlike Java, Scala uses `==` to compare, which delegates to the `equals` method. Any implementation of the `equals` method should be an **equivalence relation**, meaning:
* It is *reflexive*: `x.equals(x) == true`
* It is *symmetric*: `x.equals(y) == y.equals(x)`
* It is *transitive*: for any instance of `x`, `y`, and `z` of type `AnyRef`, if `x.equals(y) == true` and `y.equals(z) == true`, then `x.equals(z) = true`.

There are several important notes when overriding `equals` method:
* Argument type must be `Any` for Scala and `Object` for Java. Otherwise it is not overriding anything! Be sure to perform type check in the method body.
* Hashcode must be overriden as well to ensure two objects are equal with the same hashcode.
* Mutable fields should not be part of the `equals` method.

### Constructors
#### Primary Constructor
* The parameters of the primary constructor are placed immediately after the class name. These parameters become fields of the class as well.
* If no parameters are provided in class definition, this class has a primary constructor with no parameters.
* All statements in class definition belong to the primary constructor.

#### Auxiliary Constructors
They are methods named `this`. Each auxiliary constructor must call a previously defined auxiliary constructor or the primary constructor. Note fields are declared in the primary constructor.

You can often eliminate auxiliary constructors by providing default values to the primary constructor.

#### Constructors in Inheritance
Only the primary constructor can call a superclass' constructor, no matter it is primary or auxilliary. And the call is similarly intertwined into class definition:

```scala
class Employee(name: String, age: Int, val salary : Double) extends Person(name, age)
```

It calls the two-arg constructor of the `Person` class. Note that `name` and `age` are passed to the constructor of superclass, thus there is no need to declare its property like `val` or `var` again in the child class.

As it can invoke another constructor in the same class, an auxilliary constructor can never invoke a superclass constructor directly.

#### Private Primary Constructor
Sometimes e.g., to enforce the singleton pattern, the primary constructor must be made as private. Todo this, insert the `private` keyword in between the class name and any parameters of the constructor:

```scala
class Order private {} // private no-args primary constructor

class Order private (var name: String){} // private one-args primary constructor
```

To enforce the singleton pattern, put the class instance in its companion object and use a `getInstance` method to return it.

### Scope Option
Fields and methods are public by default in Scala. However, there are several scope options available:
* Object-private scope: `private[this]`. It is available only to the current instance of the current object.
* Private scope: `private`. It is available to the current class and other instances of thet current class. This is the same as `private` in Java.
* Protected scope: `protected`. In Scala, it is available to the current class and subclasses. But in Java, it is available to other classes in the same package.
* Package scope: `private[packageName]`. It is availble to all members of the current package. `packageName` can at different levels in the package hierachy, and only that level (not all subsequent levels) have access. It can also be a class name.

### Inner classes
In Java, inner classes are bouned to the outer class. In Scala, inner classes belongs to **the object of the outer class**.

You can include an object or a class inside a class or a object.

# Object
There are no static methods or fields in Scala. The `object` construct specifies a single instance of a class. It is used as *singleton*.

The constructor of an object is executed first time the object is used. However, you cannot provide constructor parameters.

An `object` can extend a class and/or one or more traits.

## Companion objects
In Java a class can have both static and non-static methods. This can be done by have a class and its companion object with the **same name** in the **same source file**.

The class and object can access each other's private members, but they're not in the same scope. Thus access is done by `ClassName.foo` or `objectName.foo` but not `foo` directly.

### The `apply` Method
`apply` serves the purpose of closing the gap between Object-Oriented and Functional paradigms in Scala. Every function in Scala can be represented as an object. Every function also has an OO type: for instance, a function that takes an Int parameter and returns an Int will have OO type of `Function1[Int,Int]`.

Now if we want to actually execute the function, or as mathematician say "apply a function to its arguments" we would call the apply method on the `Function1[Int,Int]` object.

Every function in Scala can be treated as an object and it works the other way too - every object can be treated as a function, provided it has the `apply` method.
The `apply` method: `f.apply(arg1, arg2, ...)` is the same as `f(arg1, arg2, ...)`.

The `update` method: `f(arg1, arg2, ...) = value` is the same as `f.update(arg1, arg2, ..., value)`.

Extractor: `unapply` method that retrieves values from an object. For example,

```scala
val author = "Baihu Qian"
val Name(first, last) = author // call Name.unapply(author)
// first == "Baihu", last = "Qian"
```
Note `Name` is an object.

# Inheritance
As in Java, you use `extends` keywords, and specify fields and methods that are new to the subclass or override methods in the superclass.

You can declare a class `final` so that it cannot be extended; you can declare individual methods or fields `final` so that they cannot be overriden.

In Scala you **must** provide the `override` keyword to override a method that is not abstract. This provides protection over misspelling, wrong parameter type, or name clashes. It is a good practice to use `override` keywords any time you override a concrete or abstract field, although it is not necessary for abstract fields.

Use `super` keyword to refer to the parent class. If a method is implemented in multiple traits that a class is inherited from, use `super[TraitName]` to refer to the trait. However, you cannot use this to refer to classes/traits that the class is not directly inherited from, e.g. parent of parent classes.

Scala objects cannot be inherited. Otherwise there will be multiple instances of the same object.

## Overriding
* A `def` can only override another `def`.
* A `val` can only override another `val` or a parameterless `def`.
* A `var` can only override an abstract `var`. Thus a concrete `var` sticks with the class and all its subclasses.


## Construction Order and Early Definition
Java allows constructor of superclass the call a function overridden by the subclass.

Early definition allows you to initialize `val` fields of a subclass *before* the superclass is executed.

```scala
class Ant extend {
	override val range = 2
} with Creature
```


# Abstract class
You can use `abstract` keyword to denote a class that cannot be instantiated. Unlike traits, the constructor of abstract classes can have arguments.

## Abstract methods
The abstract method does not need the `abstract` keyword. You simply omit the method body.

In child class, `override` keyword is **not** required to define the abstract method. However, an `override` keyword enforces check on method footprint to avoid typo.

## Abstract fields
It is a field without an initial value, but there is no `abstract` keywords.

Depend on it is `val` or `var`, abstract (Java sense) getter and setter methods are generated, but the field is not present in the compiled byte code.

Same to abstract methods, no override methods is required when you define a field that is abstract in the superclass.

# Traits
Like Java, Scala does not allow multiple inheritance. It is similar to Java interface as one class can extend multiple traits. It's an enhancement of Java Interface, and allows both abstract methods (like in Java) and concrete methods to be defined.

## Definition

```scala
trait Logger {
	def log(msg: String)
	def info(msg: String) { log("INFO: " + msg)
}
```
No `abstract` keywords are used for unimplemented method, thus when implementing them, no `override` keywords are necessary. Note concrete methods can refer to the abstract methods although they're not implemented yet.

Use a trait by `with` or `extend` keyword:

```scala
class Console with Logger {
	def readlog(log: String) { } // do nothing
}

class Console extends Logger {
	def readlog(log: String) { } // do nothing
}
```

Generally, class `extends` first trait, if it has no base class (otherwise it `extends` the base class), and use `with` for other traits.

Concrete classes must implement all abstract fields and methods defined in all mixed in traits, otherwise they have to be declared as `abstract`.

Contents in the trait is *mixed in* the class. They're with the class itself, not in a parent class or anywhere else.

Trait can also be mixed into objects.

## Object with Traits

Traits can be extended, and overriding concrete methods requires `override` keywords.

Objects of a class with a base trait can use child traits when instantiating themselves:

Suppose we have a Logger trait, and ConsoleLogger, FileLogger extend Logger to provide different log functions. A class, called Console, with Logger in definition. But, we can do the following:

```scala
val console1 = new Console with ConsoleLogger
val console2 = new Console with FileLogger
```

Then when log method is called, it actually refers to the implementation in ConsoleLogger or FileLogger.

## Fields
Concrete fields are fields with initial values. Concrete fields are added to the class that `with` the trait, unlike inheritance, contents of superclass is separated from those of subclass.

Abstract fields are those without initialization (thus type must be specified). When using a trait with abstract fields, they must be supplied: `val absField = 10`. Note the `val` quantifier.

It becomes handy when using object with traits:

```scala
val console1 = new Console with FileLogger {
	val filename = "log.txt"
}
```

## Layered Traits
Multiple traits are invoked from the *last one*. These traits may have a common base trait, and modify the behavoir of methods in the base trait by calling `super.someMethod`. You cannot tell from the source code with method is invoked by `super.someMethod`, it depends on the order of traits.

However, if child trait overrides an abstract method in parent traits by calling `super.someMethod` (which may be implemented by layered traits or not), the result method is still abstract. The syntax is:

	abstract override def someMethod() {
		super.someMethod(...)
	}

## Trait Construction Order
Traits can have constructor, but **no constructor parameters**. It is statements inside the trait's body.

Construction order of a class that uses the trait:

1. Constructor in the superclass
2. Trait constructor, from left to right. Within each trait, the parents get constructed first.
3. 	The class is constructed.

Be careful when initializing trait fields. If concrete fields rely on abstract fields supplied in { } after the trait name, the concrete fields are constructed **BEFORE** abstract fields get the values.

Two solutions:

* Use early definition: put the { } block between `new` and `with ClassName`.
* Use lazy value for the concrete fields, but it is inefficient.

### Traits Extending Classes
Traits can extend class, abstract class, or trait. Those classes become superclass of the classes that `with` the traits. In other words, **This trait can only be added to classes that extend a superclass.**

```scala
trait [TraitName] extends [SuperClass]
```
Then `TraitName` can only mixed in classes that extends `SuperClass`.

It is **NOT** a way to create multiple inheritance. In fact, if the class `with` the trait extends another unrelated class (that are not in inheritance hierachy of the class the trait extends), it is an error.

### Limit the Extensibility of Trait
To allow trait to be mixed into classes with certain base class, use *self types*: the trait starts with

```scala
this: [Type] =>
```

This is known as *self type*. For example:
```scala
trait LoggedException {
	this: Exception =>
		def log() { log(getMessage()) }
}
```

Then this trait can only be mixed into subclasses of `Exception`. Note this trait does not `extend Exception`, but it can call methods in `Exception` since we know it is there.

Similarly, to only allow a trait to be mixed into a type that has a method with a given structure:

```scala
trait LoggedException {
	this: { def getMessage() : String } =>
		def log() { log(getMessage()) }
}
```
This is known as *structural type*.
