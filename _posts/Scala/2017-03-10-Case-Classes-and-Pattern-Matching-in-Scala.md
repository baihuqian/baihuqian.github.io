---
title: Learning Scala - Case Classes and Pattern Matching in Scala
tags: Scala
---

* TOC
{:toc}

## Case classes
Case classes are primarily intended to create "immutable records" that is used in pattern-matching expressions. Scala compiler adds some syntactic conveniences to case class at compile time:
* A factory method with the name of the class, to allow `Var("x")` (like `apply()` method in companion object) instead of `new Var("x")`.
* An `unapply` method is generated, making it easy to use cadse classes in match expressions.
* All arguments in the parameter list of a case class implicitly get a `val` prefix, so they become fields and get accessor methods. If mutator is desired, explicitly declare the field as `var`.
* "Natural" implementations of methods `toString`, `hashCode`, and `equals` are added. This means `==` always compares objects of a case class structurally.
* A `copy` method is added for making modified copies.

Case classes implicitly `extends Product with Serializable`. Thus it is advisable to add these two traits to base classes and traits of case classes.

### Sealed Class
A sealed class cannot have any new subclasses added except the ones in the same file. The `sealed` keyword can be applied to traits as well, if they are base classes of case classes.

Sealed class is useful to ensure completeness of matching against case classes, as the compiler will flag missing combinations of patterns with a warning message.

## Pattern Matching
It offers a better switch:
```scala
var sign = ...
var ch: Char = ...

ch match {
	case '+' => sign = 1
	case '-' => sign = -1
	case _ => sign = 0
}
```

Like `if`, `match` is an expression, not a statement. It executes the code after `=>` when `ch` matches one of the case. It is equivalent to
```scala
sign = ch match {
	case '+' => 1
	case '-' => -1
	case _ => 0
}
```

`case _` is equvalent to `default` in Java switch. It catches all patterns, otherwide a `MatchError` is thrown. To access the default value, give it a variable name, i.e. use `case default` instead of `case _` and the value is stored in `default` variable.

There are no fall-through problem in pattern matching, so no `break` is needed.

Patterns are always matched top-to-bottom.

### Match Multiple Conditions
Place the match conditions that invoke the same business logic on one line, separated by `|`:

```scala
i match {
    case 1 | 3 | 5 => "odd"
    case 2 | 4 | 6 => "even"
}
```

### Guard
You can have Boolean conditions in addition to the case expression.
```scala
chmatch {
	case '+' => sign = 1
	case '-' => sign = -1
	case _ if Character.isDigit(ch) => digit = Character.digit(ch, 10)
	case _ => sign = 0
}
```

Note the boolean logic after `if` keyword is **not** enclosed in parentheses.

### Variable assignment in `match`
If the `case` keyword is followed by a variable name, then the match expression is assigned to that variable. And it can be used in a guard.

Then `case _ if Character.isDigit(ch) => digit = Character.digit(ch, 10)` is equivalent to `case ch if Character.isDigit(ch) => digit = Character.digit(ch, 10)`.

### Match by Type
```scala
obj match {
	case x: Int => x
	case s: String => Integer.parseInt(s)
	case _: BigInt => Int.MaxValue
	case ObjectName => 0 // Object matches by its own name
	case _: List[_] => "List" // Note the generic type is erased so don't supply a type!
	case _: Map[_,_] => "Map"
	case _ => 0
}
```

In scala this form is **preferred** over using `isInstanceOf` operator, and x in matching is guaranteed to have a certain type, so no need to have expensive type conversion `asInstanceOf`.

Note, because types are erased in JVM, generics must match against the generic type. However, arrays are not erased.

### Extract variables from Arrays, Lists, and Tuples using `match`
Variable binding gives you easy access to parts of a complex structure, so this operation is called *destructuring*.

#### Arrays
Use `Array` expression:
```scala
arr match {
	case Array(0) => 0 // matches array with a single zero
	case Array(x, y) => x + y // matches array with two elements, and it binds them to x and y
	case Array(0, _*) => 1 // matches array starting with zero
	case _ => -1
}
```

But in `case Array(0, _*)`case, the matched pattern cannot be accessed. To bind the matched pattern to a variable, use the following variable-binding pattern:

```scala
arr match {
    case array @ Array(0, _*) => s"$array"
}
```

#### Lists
Use `List` expression in the same way, or use the `::` operator:
```scala
list match {
	case 0 :: Nil => 0
	case x :: y:: Nil => x + y
	case 0 :: rest => 1 // get the first element
	case List(0, _, _) => 0 // match a list of length 3, starting with 0
	case List(0, _*) => 0 // match a list starting with 0
	case Nil => // match the end of list 
	case _ => -1
}
```

Again, use variable-binding pattern `@` to match the 4th and 5th cases.

#### Tuples
Use the tuple notation in the pattern:
```scala
pair match {
	case (0, _) => 0
	case (y, 0) => y
	case _ => -1
}
```

## Pattern Matching with Case Classes
Case classes can be used to match values of some fields for this particular case class, or decompose case classes into variables.
```scala
expr match {
	case CaseClass1(_, _) => // match by type
	case CaseClass2(1, _) => // match by value
	case CaseClass3(a, b) => // decompose into variable a and b
}
```
However, to match case class type, you have to write `(_, _, ...)` for all parameters in this case class. If it becomes ugly, consider using type matching instead.