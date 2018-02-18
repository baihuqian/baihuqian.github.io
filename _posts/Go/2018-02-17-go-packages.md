---
layout: "post"
title: "GoLang: Packages"
date: "2018-02-17 19:56"
tags: Go
---

# Package
Every Go program is made up of packages. Programs start running in package `main`.

By convention, the package name is the same as the last element of the import path. For instance, the "`math/rand`" package comprises files that begin with the statement `package rand`, in `$GOPATH/src/math/rand` folder`.

`math/rand` will not be accessible in `math` package unless imported.

Packages are given lower case, single-word names. The importer of a package will use the name to refer to its contents, so exported names in the package can use that fact to avoid stutter. For instance, the buffered reader type in the `bufio` package is called `Reader`, not `BufReader`, because users see it as `bufio.Reader`, which is a clear, concise name.

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

When a package is imported, the package name becomes an accessor for the contents. After
```go
import "bytes"
```
the importing package can talk about `bytes.Buffer`.

Go is strict about importing packages. It will not compile if you import a package but don't use it.

Cyclical imports are not allowed and will result in compile-time error.

# Exported names

In Go, a name is exported if it begins with a capital letter. For example, `Pizza` is an exported name, as is `Pi`, which is exported from the math package.

`pizza` and `pi` do not start with a capital letter, so they are not exported.

When importing a package, you can refer only to its exported names. Any "unexported" names are not accessible from outside the package.

# Download Libraries
`go get` command can fetch third-party libraries. If it is invoked in a project, it will scan all the fiels, looking for `imports` to third-party libraries and will download them.
