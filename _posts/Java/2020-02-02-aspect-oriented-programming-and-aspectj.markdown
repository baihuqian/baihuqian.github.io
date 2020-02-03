---
layout: post
title: Aspect-Oriented Programming and AspectJ
date: '2020-02-02 23:35'
tags: Java
---

Aspect-oriented programming (AOP) is a programming paradigm that aims to increase modularity by allowing the separation of *cross-cutting concerns*. It does so by adding additional behavior to existing code (an advice) without modifying the code itself, instead separately specifying which code is modified via a "pointcut" specification. This allows behaviors that are not central to the business logic (such as logging) to be added to a program without cluttering the code, core to the functionality.

## Example and Motivation
Typically, an aspect is scattered or tangled over the entire code base, making it harder to understand and maintain. It is scattered by virtue of the function (such as logging and metrics) being spread over a number of unrelated functions that might use its functionality. That means to change logging can require modifying all affected modules. Aspects become tangled not only with the mainline function of the systems in which they are expressed but also with each other. That means changing one concern entails understanding all the tangled concerns or having some means by which the effect of changes can be inferred.

For example, consider a banking application with a conceptually very simple method for transferring an amount from one account to another:

```java
void transfer(Account fromAcc, Account toAcc, int amount) throws Exception {
  if (fromAcc.getBalance() < amount)
      throw new InsufficientFundsException();

  fromAcc.withdraw(amount);
  toAcc.deposit(amount);
}
```

However, this transfer method overlooks certain considerations that a deployed application would require: it lacks security checks to verify that the current user has the authorization to perform this operation; a database transaction should encapsulate the operation in order to prevent accidental data loss; for diagnostics, the operation should be logged to the system log, etc.

A version with all those new concerns, for the sake of example, could look somewhat like this:

```java
void transfer(Account fromAcc, Account toAcc, int amount, User user,
    Logger logger, Database database) throws Exception {
  logger.info("Transferring money...");

  if (!isUserAuthorised(user, fromAcc)) {
    logger.info("User has no permission.");
    throw new UnauthorisedUserException();
  }

  if (fromAcc.getBalance() < amount) {
    logger.info("Insufficient funds.");
    throw new InsufficientFundsException();
  }

  fromAcc.withdraw(amount);
  toAcc.deposit(amount);

  database.commitChanges();  // Atomic operation.

  logger.info("Transaction successful.");
}
```

In this example other interests have become tangled with the basic functionality (sometimes called the business logic concern). Transactions, security, and logging all exemplify cross-cutting concerns.

Now consider what happens if we suddenly need to change (for example) the security considerations for the application. In the program's current version, security-related operations appear scattered across numerous methods, and such a change would require a major effort.

Aspect-oriented programming is a way of modularizing crosscutting concerns much like object-oriented programming is a way of modularizing common concerns. AspectJ is an implementation of aspect-oriented programming for Java.

AspectJ adds to Java just one new concept, a *join point* -- and that's really just a name for an existing Java concept. It adds to Java only a few new constructs: *pointcuts*, *advice*, *inter-type declarations* and *aspects*. Pointcuts and advice dynamically affect program flow, inter-type declarations statically affects a program's class hierarchy, and aspects encapsulate these new constructs.

A join point is a well-defined point in the program flow. A pointcut picks out certain join points and values at those points. A piece of advice is code that is executed when a join point is reached. These are the dynamic parts of AspectJ.

AspectJ also has different kinds of inter-type declarations that allow the programmer to modify a program's static structure, namely, the members of its classes and the relationship between classes.

AspectJ's aspect are the unit of modularity for crosscutting concerns. They behave somewhat like Java classes, but may also include pointcuts, advice and inter-type declarations.

## Join Point Models

The advice-related component of an aspect-oriented language defines a join point model (JPM). A JPM defines three things:

1. When the *advice* can run. These are called *join points* because they are points in a running program where additional behavior can be usefully joined. A join point needs to be addressable and understandable by an ordinary programmer to be useful. It should also be stable across inconsequential program changes in order for an aspect to be stable across such changes. Many AOP implementations support method executions and field references as join points.
1. A way to specify (or quantify) join points, called pointcuts. Pointcuts determine whether a given join point matches. Most useful pointcut languages use a syntax like the base language (for example, AspectJ uses Java signatures) and allow reuse through naming and combination.
1. A means of specifying code to run at a join point. AspectJ calls this advice, and can run it before, after, and around join points. Some implementations also support things like defining a method in an aspect on another class.

Join-point models can be compared based on the join points exposed, how join points are specified, the operations permitted at the join points, and the structural enhancements that can be expressed.

## Terminology
### Cross-Cutting Concerns
Even though most classes in an OO model will perform a single, specific function, they often share common, secondary requirements with other classes. For example, we may want to add logging and metrics to data-access layer or core business logic. Even though each class has a very different primary functionality, the code needed to perform the secondary functionality is often identical.

Cross-cutting concerns are aspects of a program that affect other concerns. These concerns often cannot be cleanly decomposed from the rest of the system in both the design and implementation, and can result in either scattering (code duplication), tangling (significant dependencies between systems), or both.

### Advice
Advice describes a class of functions which modify other functions when the latter are run; it is a certain function, method or procedure that is to be applied at a given join point of a program. The practical usage is to add logging and metrics to existing business logic.

### Pointcut
A pointcut is a set of join points. Pointcut specifies where exactly to apply advice, this allows separation of concerns and helps in modularizing business logic. Pointcuts are often specified using class names or method names in some cases using regular expressions that match class or method name. AspectJ syntax is considered as de facto standard.

### Aspect
An aspect of a program is a feature linked to many other parts of the program, but which is not related to the program's primary function. An aspect crosscuts the program's core concerns, therefore violating its separation of concerns that tries to encapsulate unrelated functions. For example, logging code can crosscut many modules, yet the aspect of logging should be separate from the functional concerns of the module it cross-cuts. In other words, the combination of the advice and pointcut is an aspect.

## AspectJ
AspectJ is a general-purpose aspect-oriented extension to Java.

### Pointcut
AspectJ provides for many kinds of *join points*, *pointcuts* pick out certain join points in the program flow. For example, the pointcut

```java
call(void Point.setX(int))
```

picks out each join point that is a call to a method that has the signature `void Point.setX(int)` — that is, `Point`'s `void setX` method with a single `int` parameter. A pointcut can be built out of other pointcuts with and, or, and not (spelled `&&`, `||`, and `!`).

AspectJ allows programmers to define their own named pointcuts with the pointcut form. So the following declares a new, named pointcut:

```java
pointcut move():
    call(void FigureElement.setXY(int,int)) ||
    call(void Point.setX(int))              ||
    call(void Point.setY(int))              ||
    call(void Line.setP1(Point))            ||
    call(void Line.setP2(Point));
```

### Advice
To actually implement crosscutting behavior, we use *advice*. Advice brings together a pointcut (to pick out join points) and a body of code (to run at each of those join points).

AspectJ has several different kinds of advice, such as *before*, *after* (three kinds, *after returning*, *after throwing*, and plain *after* which runs after returning or throwing, like Java's finally), and *around*. For example,

```java
before(): move() {
   System.out.println("about to move");
}

after() returning: move() {
   System.out.println("just successfully moved");
}
```

Pointcuts not only pick out join points, they can also expose part of the execution context at their join points. Values exposed by a pointcut can be used in the body of advice declarations.

An advice declaration has a parameter list (like a method) that gives names to all the pieces of context that it uses. For example,

```java
after(FigureElement fe, int x, int y) returning:
       ...SomePointcut... {
   ...SomeBody...
}
```

### Inter-type declarations
Inter-type declarations in AspectJ are declarations that cut across classes and their hierarchies. They may declare members that cut across multiple classes, or change the inheritance relationship between classes. Unlike advice, which operates primarily dynamically, introduction operates statically, at compile-time.

AspectJ can express the concern in one place using inter-type declarations. The aspect declares the methods and fields that are necessary to implement the new capability, and associates the methods and fields to the existing classes.

For example,
```java
aspect PointObserving {
    // Add a private field to Point class
    private Vector Point.observers = new Vector();

    // Add static methods to manipulate the private field
    public static void addObserver(Point p, Screen s) {
        p.observers.add(s);
    }
    public static void removeObserver(Point p, Screen s) {
        p.observers.remove(s);
    }

    // Define pointcut and advice that utilize the private field
    pointcut changes(Point p): target(p) && call(void Point.set*(int));

    after(Point p): changes(p) {
        Iterator iter = p.observers.iterator();
        while ( iter.hasNext() ) {
            updateObserver(p, (Screen)iter.next());
        }
    }

    static void updateObserver(Point p, Screen s) {
        s.display(p);
    }
}
```

### Aspect
Aspects wrap up pointcuts, advice, and inter-type declarations in a a modular unit of crosscutting implementation. It is defined very much like a class, and can have methods, fields, and initializers in addition to the crosscutting members. Because only aspects may include these crosscutting members, the declaration of these effects is localized.

Like classes, aspects may be instantiated, but AspectJ controls how that instantiation happens -- so you can't use Java's `new` form to build new aspect instances. By default, each aspect is a singleton, so one aspect instance is created. This means that advice may use non-static fields of the aspect, if it needs to keep state around.

## References
1. [Wikipedia](https://en.wikipedia.org/wiki/Aspect-oriented_programming)
