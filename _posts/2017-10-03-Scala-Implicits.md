---
title: Learning Scala - Implicits
tags: Scala
---

* TOC
{:toc}

`implicit` keyword provides powerful boilerplate-reduction functionalities in Scala. Type conversions, passing global paramters to methods, etc., can be automatically done by compiler with implicits.

# Implicit Class
Scala 2.10 introduced a new feature called implicit classes. An *implicit* class is a class marked with the `implicit` keyword. This keyword makes the class’ primary constructor available for implicit conversions when the class is in scope.

**Implicit class wraps another class and provide new methods to that class.** For example,

```scala
object Helpers {
  implicit class IntWithTimes(x: Int) {
    def times[A](f: => A): Unit = {
      def loop(current: Int): Unit =
        if(current > 0) {
          f
          loop(current - 1)
        }
      loop(x)
    }
  }
}
```
It provides a `times` method to `Int`.

Restrictions:
1. They must be defined inside of another `trait/class/object`.
2. They may only take one non-implicit argument in their constructor.
3. There may not be any method, member or object in scope with the same name as the implicit class.

To satisfy restriction 1, one can put it in an Object, and import everything of the object into scope; or put it in a package object, and import everything from that package.

It is recommended to **explicitly specify** the return value of the methods in the implicit class.

# Implicit Parameters
An implicit parameter is just a function parameter annotated with the `implicit` keyword. A method with *implicit* parameters can be applied to arguments just like a normal method. However, if such a method misses arguments for its implicit parameters, such arguments will be automatically provided.

The actual arguments that are eligible to be passed to an implicit parameter fall into two categories:
* First, eligible are all visible identifiers that are marked as `implicit`. It can be `val`, `var`, or `def` that yields a value of parameter's type.
* Second, eligible are also all members of companion modules of the implicit parameter’s type that are labeled `implicit`. Normally, they are implicit members of the implicit parameter's type in the companion object.

However, you can’t have more than one in scope.

You can only use implicit once in a parameter list and all parameters following it will be implicit.

# Implicit Functions
Implicit functions will be called automatically if the compiler thinks it’s a good idea to do so. What that means is that if your code doesn’t compile but would, if a call was made to an implicit function, Scala will call that function to make it compile. They’re typically used to create *implicit conversion* functions; single argument functions to automatically convert from one type to another.

If a second argument is required for implicit functions, it should be marked as `implicit` and passed as implicit parameters via currying. For example,

```scala
implicit def toBase(x: Int)(implicit base: Int): String = ???

implicit val base = 2

val binaryString: String = 2 // implicitly convert to 2-based string representation
```