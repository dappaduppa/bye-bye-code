---
layout: post
title:  "Java Generics faq"
date:   2017-08-28 11:45:46 +0900
categories: Java, Generics, Angelika
---

Definition:
-----------

**List`<`E`>`**  

**List`<`String`>`**

    1) E = type parameter
    2) String = actual type argument
    3) List<String> = parameterized type

why go-lang not used generics, and how it survives without generics.


## Qn and Ans

## Qn 1:
```java
///

package so.generics;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Cast {

    public static void main(String[] args) {
        List<String> list = Arrays
                .asList(new String[] { "abcd", "efgh", null });
        List<String> list2 = new ArrayList<>(list);

        list.remove("abcd");
        System.out.println(list2);
    }

}
```

-----------------------------

## Qn 2:
```java
///
From the generic type only Known, create instance
```
-----------------------------

## Qn 3:
```java
///

interface Animal {

}

class Dog extends Animal {

}

List<Animal> animals = new ArrayList<>();
animals.add(new Dog()); // OK?
```
-----------------------------

## Qn 4:
```java
package ddd.serial;

import java.util.ArrayList;
import java.util.List;

public class GenericWildCard {
    public static void main(String[] args) {
        final List<?>[] lsa = new List<?>[4];
        lsa[0] = new ArrayList<String>();
        lsa[1] = new ArrayList<Integer>();
        lsa[0].add("323");
        lsa[0] = lsa[1];

        System.out.println(lsa[1]);

    }
}
```

-----------------------------

## ans:
----

## Ans 1:
--------------------------------------------------------------------------------
```java
--------------------------------------------------------------------------------
Exception in thread "main" java.lang.UnsupportedOperationException
    at java.util.AbstractList.remove(AbstractList.java:161)
    at java.util.AbstractList$Itr.remove(AbstractList.java:374)
    at java.util.AbstractCollection.remove(AbstractCollection.java:283)
    at so.generics.Cast.main(Cast.java:14)
```

--------------------------------------------------------------------------------
## Ans 2:
--------------------------------------------------------------------------------
```java

package so.generics;

public class Foo<T> {

    public T createInst(Class<T> type) throws InstantiationException,
            IllegalAccessException {
        return type.newInstance();
    }

    public static void main(String[] args) throws InstantiationException,
            IllegalAccessException {
        System.out.println(new Foo<String>().createInst(String.class));
    }

}
```
--------------------------------------------------------------------------------

## Ans 3:
--------------------------------------------------------------------------------
```java

In List<Animal> , "dog" instance can be added.

but

List<Dog> dogs = ...
List<Animal> animals = dogs; // NOT Valid
```
--------------------------------------------------------------------------------

## Ans 4:
--------------------------------------------------------------------------------
```java

Complation err on
 lsa[0].add("323");

 because can t add value to unknown ( ? type) type.
```
--------------------------------------------------------------------------------

----------------

## more q and a
```java
static <T> void fromArrayToCollection(T[] a, Collection<? extends T> c) {
    for (T t : a) {
        c.add(t); //result NOT OK
    }
}

static <T> void fromArrayToCollection(T[] a, Collection<T> c) {
    for (T t : a) {
        c.add(t); //result OK
    }
}


Object[] oa = new Object[100];
Collection<Object> co = new ArrayList<Object>();

//result ok T inferred to be Object
fromArrayToCollection(oa, co);

String[] sa = new String[100];
Collection<String> cs = new ArrayList<String>();

//result ok T inferred to be String
fromArrayToCollection(sa, cs);

//result ok T inferred to be Object
fromArrayToCollection(sa, co);

Integer[] ia = new Integer[100];
Float[] fa = new Float[100];
Number[] na = new Number[100];
Collection<Number> cn = new ArrayList<Number>();

//result ok T inferred to be Number
fromArrayToCollection(ia, cn);

//result ok T inferred to be Number
fromArrayToCollection(fa, cn);

//result ok T inferred to be Number
fromArrayToCollection(na, cn);

//result ok T inferred to be Object
fromArrayToCollection(na, co);

//result NOT OK compile-time error
fromArrayToCollection(na, cs);

---------
//why bounded wildcard in the following is better

    interface Collection<E> {
        public boolean containsAll(Collection<?> c);
        public boolean addAll(Collection<? extends E> c);
    }
//than generic methods.?

//result in both containsAll and addAll,
//result 1) the type parameter T is used only once.
//result 2) The return type doesn't depend on the type parameter,
//result   nor does any other argument to the method
//result   (in this case, there simply is only one argument).

//result . Wildcards are designed to support flexible subtyping

//result Generic methods allow type parameters to be used to express dependencies
//result among the types of one or more arguments to a method and/or its return type.
//result If there isn't such a dependency, a generic method should not be used.


interface Collection<E> {
    public <T> boolean containsAll(Collection<T> c);
    public <T extends E> boolean addAll(Collection<T> c);
    // Hey, type variables can have bounds too!
}
```

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/