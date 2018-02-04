---
title: Learning Scala - Application
tags: Scala
---

An object that extends the `App` trait becomes the entry point of a Scala application, and the body of the object will be run. Any command-line arguments will be available through the `args` object of type `Array[String]`, which is inherited from the `App` trait.

An object that contains a `main(args: Array[String])` method. It is similar to Java.

## Execution of shell commands
Use `scala.sys.process` package.

Put commands in quotes and append `!` or `!!`: `"ls -al .." !`

* `!`: return value is the exit code;
* `!!`: return value is the actual output of the command.

Prefix # to the following operators:

* `|` is supported, use `#|`. All commands are in quotes.
* Redirect output (`>`) using `#>`.
* Append to a file (`>>`) using `#>>`.
* Redirect input (`<`) using `#<`.
* Execute q if p was successful: `p #&& q`.
* Execute q if p was unsuccessful: `p #|| q`.

