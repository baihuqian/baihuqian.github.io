---
layout: "post"
title: "Try-with-Resource Statements in Java"
date: "2020-01-31 09:42"
tags: Java
---

There are many resources, such as file handles, sockets, or buffers, must be closed after use to avoid resource leakage. It is easy to forget to do so or implement it in an incorrect way. In Python, the `with` statement simplifies the management of closable resources by automatically invoking it's `close()` method when exiting the scope of the `with` statement. In Java, you need to do something like

```java
BufferedReader br = new BufferedReader(new FileReader(path));
try {
    return br.readLine();
} finally {
    if (br != null) br.close();
}
```
This is tedious and error prone. In Java SE 7, the try-with-resource statement is introduced to simplify it.

The `try`-with-resources statement is a `try` statement that declares one or more resources. A resource is an `object` that must be closed after the program is finished with it. The `try`-with-resources statement ensures that each resource is closed at the end of the statement. Any object that implements `java.lang.AutoCloseable`, which includes all objects which implement `java.io.Closeable`, can be used as a resource. The previous example can be simplified with

```java
try (BufferedReader br =
               new BufferedReader(new FileReader(path))) {
    return br.readLine();
}

```

You may declare one or more resources in a try-with-resources statement. Multiple resources are separated with `;`, like this:

```java
try (
    java.util.zip.ZipFile zf =
         new java.util.zip.ZipFile(zipFileName);
    java.io.BufferedWriter writer =
        java.nio.file.Files.newBufferedWriter(outputFilePath, charset)
) {
    // Enumerate each entry
    for (java.util.Enumeration entries =
                            zf.entries(); entries.hasMoreElements();) {
        // Get the entry name and write it to the output file
        String newLine = System.getProperty("line.separator");
        String zipEntryName =
             ((java.util.zip.ZipEntry)entries.nextElement()).getName() +
             newLine;
        writer.write(zipEntryName, 0, zipEntryName.length());
    }
}
```

When the block of code that directly follows it terminates, either normally or because of an exception, the close methods of all resources are automatically called in the **opposite** order of their creation (last-in-first-out).

**Note**: A try-with-resources statement can have `catch` and `finally` blocks just like an ordinary try statement. In a try-with-resources statement, any `catch` or `finally` block is run after the resources declared have been closed.

**Note**: If an exception is thrown from the `try` block and one or more exceptions are thrown from the `try`-with-resources statement, then those exceptions thrown from the `try`-with-resources statement are suppressed. Only exception thrown from the `try` block is thrown out.
