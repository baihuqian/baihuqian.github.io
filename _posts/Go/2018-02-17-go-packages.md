---
layout: "post"
title: "GoLang: Packages"
date: "2018-02-17 19:56"
tags: Go
---

# Package
Every Go program is made up of packages. Programs start running in package `main`.

By convention, the package name is the same as the last element of the import path. For instance, the "`math/rand`" package comprises files that begin with the statement package `rand`.

## Imports
You can import one package per line, such as

```go
import "fmt"
import "math"
```
But the convention is to use "factored" import statement:

```go
import (
	"fmt"
	"math"
)
```

Go is strict about importing packages. It will not compile if you import a package but don't use it.

# Exported names

In Go, a name is exported if it begins with a capital letter. For example, `Pizza` is an exported name, as is `Pi`, which is exported from the math package.

`pizza` and `pi` do not start with a capital letter, so they are not exported.

When importing a package, you can refer only to its exported names. Any "unexported" names are not accessible from outside the package.
