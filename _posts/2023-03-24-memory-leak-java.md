---
layout: post
title: "How to create a memory leak in Java program?"
---
Here's a good way to create a true memory leak (objects inaccessible by running code but still stored in memory) in pure Java:
- The application creates a long-running thread (or use a thread pool to leak even faster).
- The thread loads a class via an (optionally custom) `ClassLoader`.
- The class allocates a large chunk of memory (e.g. `new byte[1000000]`), stores a strong reference to it in a static field, and then stores a reference to itself in a `ThreadLocal`  Allocating the extra memory is optional (leaking the class instance is enough), but it will make the leak work that much faster.
- The application clears all references to the custom class or the `ClassLoader` it was loaded from.
- Repeat.

Due to the way ThreadLocal is implemented in Oracle's JDK, this creates a memory leak:

- Each `Thread` has a private field threadLocals, which actually stores the thread-local values.
- Each key in this map is a weak reference to a `ThreadLocal` object, so after that `ThreadLocal` object is garbage-collected, its entry is removed from the map.
- But each value is a strong reference, so when a value (directly or indirectly) points to the `ThreadLocal` object that is its key, that object will neither be garbage-collected nor removed from the map as long as the thread lives.

In this example, the chain of strong references looks like this:

`Thread` object → `threadLocals` map → instance of example class → example class → static `ThreadLocal` field → `ThreadLocal` object.

(The `ClassLoader` doesn't really play a role in creating the leak, it just makes the leak worse because of this additional reference chain: example class → `ClassLoader` → all the classes it has loaded. It was even worse in many JVM implementations, especially prior to Java 7, because classes and `ClassLoaders` were allocated straight into permgen and were never garbage-collected at all.)

A variation on this pattern is why application containers (like Tomcat) can leak memory like a sieve if you frequently redeploy applications which happen to use `ThreadLocals` that in some way point back to themselves. This can happen for a number of subtle reasons and is often hard to debug and/or fix.

[reference](https://stackoverflow.com/questions/6470651/how-can-i-create-a-memory-leak-in-java?rq=1)
