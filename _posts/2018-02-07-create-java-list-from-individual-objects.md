---
layout: post
published: true
title: Create Java List from Individual Objects
tags: Java
---

### Problem
Sometimes, I need to create a Java List containing only one object.

### Solution
Use the static `asList` method in `java.util.Arrays` class:

```java
Arrays.asList(obj)
```

It takes a varargs parameter so I can provide multiple objects. The JavaDoc for this API says:

>```Java
> public static <T> List<T> asList(T... a)
> ```

>Returns a fixed-size list backed by the specified array. (Changes to the returned list "write through" to the array.) This method acts as bridge between array-based and collection-based APIs, in combination with `Collection.toArray()`.
