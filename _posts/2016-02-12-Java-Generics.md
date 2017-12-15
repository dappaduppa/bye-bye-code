---
layout: post
title:  "Java Generics"
date:   2016-02-12 09:36:46 +0900
categories: Java Generics
---

## Inbuilt generic types
```java
Generic type
	LinkedList<E>
Parameterized type
	LinkedList<String>
/*
type parameter
	E
Actual type argument
	String

Generic type => type with formal parameters.
Parameterized type => instantiation of generic type with actual parameters.
*/
```

c++ template vs "java" generics

http://stackoverflow.com/questions/36347/what-are-the-differences-between-generic-types-in-c-and-java

	c++ template --> at compile time for each type separate code generated.  
	java     -->  type erased by "type erasure"  

## Generics need and implementation:
```java
* for efficient implementation of "homogenious collections"
* generics implemented NOT only collection classes, but also in
    java.lang.ref
        WeakReference
        SoftReference
    ( study reflection in detail )
    java.util.Concurrent
        Callable
            V call() throws Exception;
    Class<T>
        denotes the Type that the class object represents.
```

## Benefit of generics:
```java
Early error detection @ compile time.
    i.e. type safety 

Type safety means:
No ClassCastException at runtime.
```
