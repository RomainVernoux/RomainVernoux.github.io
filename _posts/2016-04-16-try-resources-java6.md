---
layout: post
title: "A try-with-resources equivalent for Java 6 and Android"
date: 2016-04-16 23:20:03 +0100
categories: java android english
---
In Java core libraries, resources such as network, files and databases can be accessed through a large range of low-level interfaces which all require to explicitly handle exceptions and release resources properly.
As a consequence, writing such code is arguably more error-prone than in languages with high-level resource access methods, and it is actually quite common to find bugs related to flawed resource management in Java codebases.

To tackle this issue, Java 7 offers higher-level methods to access files in the `java.nio.file` package, but also a new *try-with-resources* language construct to automatically release resources, even when errors occur.
Unfortunately, these new features are not available for all and some projects are still stuck with Java 6.
In Android, support for *try-with-resources* was added in SDK 19 (Android 4.4), therefore apps compatible with earlier versions of the system cannot use it.

In this article I will show why properly releasing resources can get tricky in the presence of errors, show Java 7's *try-with-resources* solution, then explain how it is mostly syntactic sugar for some Java 6 code.
The takeaway is a general-purpose code snippet to start from and simplify, in order to get resource management right everytime, even in old Java environments.


## The problem

Leaving aside error handling for now, reading from a file using the `java.io` package looks like this:

{% highlight java %}
FileReader fileReader = new FileReader("/path/to/your/file");
BufferedReader bufferedReader = new BufferedReader(fileReader);
String line = null;
while ((line = bufferedReader.readLine()) != null) {
    // Handle each line
}
bufferedReader.close();
fileReader.close();
{% endhighlight %}

Two things to note here: the chained readers (lines 1-2) and the explicit release of resources (lines 7-8).
*Yes, looking at the [source code](http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/8u40-b25/java/io/BufferedReader.java?av=f#525), `BufferedReader.close()` does itself call `close()` on the wrapped `FileReader`, but this is [not clearly documented](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html#close--) and in general one should not assume that chained decorators release each other's resources properly.*
At this point it is worth saying that even if Java garbage collection will eventually call `destroy()` on both readers (which should release their resources), there is no guarantee that this will happen soon enough to avoid other parts of the program to fail accessing the same resource.

Now, this piece of code seems simple enough, but it lacks the hardest part: error handling.
In these few lines, exceptions can be thrown in multiple places:

1. Obviously, everytime the resources are used (line 4), in which case all resources should be released.
2. In the first constructor (line 1), in which case there is nothing yet to release.
3. In the chained constructors (line 2), in which case all previously opened resources should be closed. One might point out that decorator constructors usually do not throw exceptions, but this does not account for unchecked exceptions (some like `OutOfMemoryError` are not that uncommon) and critical resources like database connections really need to be released in all cases.
4. When a decorator is closed (line 7), in which case all wrapped resources still need to be released.
5. When the innermost object is closed (line 8), in which case there is nothing left to release.

In Java, one can make sure that a particular portion of the code is executed regardless of exceptions using the `finally` clause of the `try/catch/finally` construct. Point (1) above can be dealt with as follows:

{% highlight java %}
FileReader fileReader = new FileReader("/path/to/your/file");
BufferedReader bufferedReader = new BufferedReader(fileReader);
String line = null;
try {
    while ((line = bufferedReader.readLine()) != null) {
        // Handle each line
    }
} finally {
    bufferedReader.close();
    fileReader.close();
}
{% endhighlight %}

Note that this correction does not handle exceptions, it only guarantees that even if an exception occurs while reading lines, an attempt will be made to close the `BufferedReader` and the `FileReader`.
Trying to tackle point (2) - (5) above, a bigger refactoring is required:

{% highlight java %}
FileReader fileReader = new FileReader("/path/to/your/file");
try {
    BufferedReader bufferedReader = new BufferedReader(fileReader);
    String line = null;
    try {
        while ((line = bufferedReader.readLine()) != null) {
            // Handle each line
        }
    } finally {
        bufferedReader.close();
    }
} finally {
    fileReader.close();
}
{% endhighlight %}

This is getting ugly.
What if we had 3 chained decorators? And this code snippet is not even correct.
For instance, if an exception occurs while reading the file content (lines 6-8) *and* during the release of the `BufferedReader` (line 10), the entire block will throw the exception from the `finally` clause (this is Java semantics), while the failure of the block was in fact caused by the first exception.
The same can be said about closing the `FileReader` this way.
Furthermore, there is no guarantee that lines 6-8 will not re-affect variables `fileReader` and `bufferedReader` and a `NullPointerException` could be thrown on lines 10 and 13.
And keep in mind that we didn't even try to *handle* exceptions yet, but only to release resources correctly.
There must be a better way.


## Try-with-resources in Java 7

In Java 7, the `try/catch/finally` construct was extended to handle this very particular issue.
Resources declared in the first part of the `try` clause and that implement the new `AutoCloseable` interface are automatically released even when exceptions occur, meeting the five requirements above and throwing the right exception, if any:

{% highlight java %}
try (
    FileReader fileReader = new FileReader("/path/to/your/file");
    BufferedReader bufferedReader = new BufferedReader(fileReader);
) {
    String line = null;
    while ((line = bufferedReader.readLine()) != null) {
        // Handle each line
    }
}
{% endhighlight %}

That's it.
Done.


## How to do it in Java 6 and Android?

In fact, the *try-with-resources* construct is mostly syntactic sugar for valid Java 6 code.
As shown [here](http://www.oracle.com/technetwork/articles/java/trywithresources-401775.html), by decompiling the bytecode corresponding to the snippet above, one can see the following result:

{% highlight java %}
FileReader fileReader = new FileReader("/path/to/your/file");
Object obj1 = null;
try {
    BufferedReader bufferedReader = new BufferedReader(fileReader);
    Object obj2 = null;
    try {
        String line = null;
        while ((line = bufferedReader.readLine()) != null) {
            // Handle each line
        }
    } catch (Throwable th2_1) {
        obj2 = th2_1;
        throw th2_1;
    } finally {
        if (bufferedReader != null) {
            if (obj2 != null) {
                try {
                    bufferedReader.close();
                } catch (Throwable th2_2) {
                    obj2.addSuppressed(th2_2);
                }
            } else {
                bufferedReader.close();
            }
        }
    }
} catch (Throwable th1_1) {
    obj1 = th1_1;
    throw th1_1;
} finally {
    if (fileReader != null) {
        if (obj1 != null) {
            try {
                fileReader.close();
            } catch (Throwable th1_2) {
                obj1.addSuppressed(th1_2);
            }
        } else {
            fileReader.close();
        }
    }
}
{% endhighlight %}

A careful read of this piece of code shows that

- All resources are correctly released, even when exceptions (checked or unchecked) occur.
- No `NullPointerException` can be thrown when calling `close()` methods.
- Each code block eventually throws the exception that first caused the failure.

On this last point, notice how secondary exceptions are handled, calling `addSuppressed()` on the original exception (lines 20 and 36).
This method was also added to the Throwable class in Java 7.
It attaches another exception to the thrown exception and signals that it was [suppressed](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html#suppressed-exceptions) (muted) for the correct exception to be thrown.
Suppressed exceptions are printed alongside the stack trace elements in the error log with a `Suppressed:` tag.
This is different from nested exceptions, which represent a causality relation between exceptions in the stacktrace and are printed with a `Caused by:` tag in the error log.

Except for these two calls, this is pure Java 6 code and should be considered the *correct* way to implement resource access in Java before version 7.
For the suppressed exceptions, one has two options: (i) do nothing, the suppressed exception could simply be ignored and not be printed in the stack trace or (ii) implement special handling of suppressed expressions e.g., throw a new custom exception "caused by" the original expression and storing a list of suppressed exceptions.


## Conclusion: a general solution

Building on these observations, the correct Java 6 equivalent for a general *try-with-resources* statement

{% highlight java %}
try (
    Resource1 resource1 = new Resource1();
    Resource2 resource2 = new Resource2(resource1);
    ...
    ResourceN resourceN = new ResourceN(resourceN-1);
) {
    // Use resourceN
}
{% endhighlight %}

is as follows:

{% highlight java %}
Resource1 resource1 = new Resource1();
Object obj1 = null;
try {
    Resource2 resource2 = new Resource2(resource1);
    Object obj2 = null;
    try {
        // ...
         // ...
          // ...
            ResourceN resourceN = new ResourceN(resourceN-1);
            Object objN = null;
            try {
                // Use resourceN
            } catch (Throwable thN_1) {
                objN = thN_1;
                throw thN_1;
            } finally {
                if (resourceN != null) {
                    if (objN != null) {
                        try {
                            resourceN.close();
                        } catch (Throwable thN_2) {
                            // Handle suppressed thN_2
                        }
                    } else {
                        resourceN.close();
                    }
                }
            }
          // ...
         // ...
        // ...
    } catch (Throwable th2_1) {
        obj2 = th2_1;
        throw th2_1;
    } finally {
        if (resource2 != null) {
            if (obj2 != null) {
                try {
                    resource2.close();
                } catch (Throwable th2_2) {
                    // Handle suppressed th2_2
                }
            } else {
                resource2.close();
            }
        }
    }
} catch (Throwable th1_1) {
    obj1 = th1_1;
    throw th1_1;
} finally {
    if (resource1 != null) {
        if (obj1 != null) {
            try {
                resource1.close();
            } catch (Throwable th1_2) {
                // Handle suppressed th1_2
            }
        } else {
            resource1.close();
        }
    }
}
{% endhighlight %}

It might not look great, but it is the right way.
Notice that if you don't mind dismissing the suppressed exceptions and if you know for sure that `resourceN` objects are never set to `null`, this template code can be greatly simplified:

{% highlight java %}
Resource1 resource1 = new Resource1();
try {
    Resource2 resource2 = new Resource2(resource1);
    try {
        // ...
         // ...
          // ...
            ResourceN resourceN = new ResourceN(resourceN-1);
            try {
                // Use resourceN
            } finally {
                try { resourceN.close(); } catch (Throwable thN_2) {}
            }
          // ...
         // ...
        // ...
    } finally {
        try { resource2.close(); } catch (Throwable th2_2) {}
    }
} finally {
    try { resource1.close(); } catch (Throwable th1_2) {}
}
{% endhighlight %}

Et voilà.
