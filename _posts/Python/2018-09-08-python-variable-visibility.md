---
layout: "post"
title: "Python Notes: Variable Visibility"
date: "2018-09-08 16:04"
tags: Python
---

# Global Variable
In Python, a variable declared outside of the function or in global scope is known as global variable. This means, global variable can be accessed (read) inside or outside of the function.

```python
x = "global"

def foo():
    print("x inside :", x)

foo()
print("x outside:", x)
```

However, global variables cannot be modified inside functions where it is visible (i.e. can be accessed) without the `global` declaration:

```python
x = "global"

def foo():
    x = x * 2
    print(x)
foo()

# UnboundLocalError: local variable 'x' referenced before assignment
```

`global` keyword allows you to modify the variable outside of the current scope. It is used to create a `global` variable and make changes to the variable in a local context.

The basic rules for global keyword in Python are:

* When we create a variable inside a function, it’s local by default.
* When we define a variable outside of a function, it’s global by default. You don’t have to use `global` keyword.
* We use `global` keyword to write to a global variable inside a function.
* Use of `global` keyword outside a function has no effect.
* In nested functions, `global` keyword declared in inner functions does not have effect in the outer function.

# Local Variable
A variable declared inside the function's body or in the local scope is known as local variable. Local variables are only visible to the function scope. If a local variable is defined with the same name as a global variable, the global variable can no longer be accessed from the scope.

# Nonlocal Variable
Python 3 introduces the `nonlocal` keyword that allows you to assign to variables in an outer, but non-global, scope.

```python
def outside():
  msg = "Outside!"
  def inside():
      msg = "Inside!"
      print(msg)
  inside()
  print(msg)

```

Without `nonlocal` keyword, Python creates a new variable called `msg` in the local scope of `inside` that shadows the name of the variable in the `outer` scope. Now, by adding nonlocal msg to the top of inside, Python knows that when it sees an assignment to msg, it should assign to the variable from the outer scope instead of declaring a new variable that shadows its name.

```python
def outside():
    msg = "Outside!"
    def inside():
        nonlocal msg
        msg = "Inside!"
        print(msg)
    inside()
    print(msg)
```

Note, `nonlocal` keyword only affects **assignment**. You can still modify variables in the outer scope by function calls.
