---
layout: "post"
title: "Kotlin for Java Developers"
date: "2020-08-23 16:38"
---

I use Scala as the day-to-day programming language. However, I would acknowledge that Scala is not the easiest language to pick up and master. While Java is a good language with unbeatable ecosystem, some of its language designs, due to backward compatibility reasons, are not great. To name a few:
1. Primitive types and their boxed object types.
2. Null handling and NullPointerException.
3. Type parameters, collections, and type erasure.
4. Array is different from collections.
5. No first-class functional programming support, and functional interface in Java is mostly a hack.
6. Amount of boilerplate code required.

In addition, while Java has been adopting new language features to mitigate certain aspects, the fast evolution of language version and restrictive license from Oracle does not improve the adoption of new Java versions into production systems. A lot of systems are still in Java 8. Therefore, I'm interested in Kotlin/JVM when it first came out and the later adoption by Android. This is mostly the notes I took when studying [Kotlin for Java Developers](https://www.coursera.org/learn/kotlin-for-java-developers/).

# Basics
Kotlin can work with existing Java code in the same project. Not only the Kotlin code can reference existing Java code, but the Java code can invoke Kotlin code as well. Kotlin provides a tool to automatically transform Java code into Kotlin.
