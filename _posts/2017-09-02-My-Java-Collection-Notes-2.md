---
layout: post
title:  "My Java Collection Notes 2"
date:   2017-09-02 07:28:46 +0900
categories: Java, Collection
---

## Collection<?> unknown type collection can not be added.
```java
Since any element can not be unknown type.
Compile time error is thrown.

    Collection<?> coll = new ArrayList<String>();
    coll.add("abcd");// compilation err is thrown.
                     // but "null" can be added. since "null" is
                     // member of every type.

    coll.remove("abcd"); // OK!


But, you can remove the items from collections.!

```

## integer collections.
```java
Collection<Integer> coll = new ArrayList<Integer>();
coll.add(12345);// OK!
coll.remove(12345); // OK
for (Integer integer : coll) {
    System.out.println(integer);
}
```
/*******************************************************************************/

## Generic class, and interfaces => (called as) geeneric types

### generics faq:
```java
LinkedList<E> ==> generic type.

E => type parameter / formal type parameter

LinkedList<String>  -->  parameterized type

LinkedList<Integer> -->  parameterized type

String  --> actual type argument
Integer --> actual type argument
```

## Type parameter naming connventions:
```java
1) Single char. if possible.
2) Avoid lower case.
3) Most container types use E
```

## Collections unknown type ?
```java
import java.util.ArrayList;
import java.util.Collection;
import java.util.List;


public class MyUnMod {

    public static void main(String[] args) {


        final List<String> list = new ArrayList<String>();
        list.add("one");
        list.add("two");
        list.add("three");
        list.add("four");

        System.out.println(">>>MyUnMod=" + list);

        Collection<?> coll = list;
        list.remove(123); // at runtime IndexOutOfBoundsException: Index: 123, Size: 4
                          //  (because it operates on the list of "String")
        coll.remove(123); // but this one no error/exception at runtime, since it is promoted to Object I guess.
                        // ( because it operates on the collection of "unknown" objects. )
        coll.add(null);

        System.out.println(">>>MyUnMod=" + list);
    }
}


Also note that, for creating unmodifiable collection

    assign to Collection<?> , "does NOT" solve the requirement.

because we can add null, and as well when we get elements we have to cast to the
required type.


but if we do this,

        Collection<? extends String> coll = list; // wow I extended "String"

        for (final String s: coll ) { // no casting required to "String"
            System.out.println(">>>s=" + s);
        }

? => unknown type
List<? extends Shape> is an example of bounded wildcard

```

## Unknown type qualities:
```java
1) canT add (except null)
2) can remove.
3) can not assign value to the var. (in that bounded wildcards
like "? extends String" have to be used.)
please refer the following example.

/*******************************************************************************/
import java.util.LinkedList;
import java.util.List;


public class HWorld {

    public static void main(String[] args) {

        List<String> list = new LinkedList<>(); // 1
        list.add("one");
        list.add("two");
        list.add("three");

        List<? extends String> list2 = list;

        //list2.remove("four");

        String s1 = list2.get(2);
        System.out.println(s1);

        //Integer x = (Integer)myIntList.iterator().next(); // 3
    }

}

```

## bounded wildcard sample

```java
CustomerAccount
    SavingsAccount
    CurrentAccount

EBContractBalances ecb


 // ? extends CustomerAccount is used,
 // NO need to check ca.checkType() == "CA" or "SA" is required
 // the passed instance checkBalance is invoked., in built in the langauage.
public void checkBalance(List<? extends CustomerAccount> custAccounts) {
    for(final CustomerAccount ca: custAccounts) {
        ecb.checkBalance(ca);
    }

}


previously it is ....separate for diff. accounts


public void checkBalance(List<SavingsAccount> saAccounts) {
    for(final SavingsAccount sa: saAccounts) {
        ecb.checkBalance(sa);
    }
}

public void checkBalance(List<CurrentAccount> currAccounts) {
    for(final CurrentAccount currAc: currAccounts) {
        ecb.checkBalance(currAc);
    }
}
```

## Generic methods :
```java
Adding elements to Collection of Object does not work.
    and as well collection of unknown type(?) also won't work.

So "Generic methods"

sample.

public static void fromArray2Collection(Object[] objArr, Collection<Object> coll) {
    for(Object object: objArr) {
        coll.add(object); // works only for Object!
    }

}
```

## wild cards vs generic methods...
```java
prefer wild cards at the max.

Interoperating with Legacy Code
-------------------------------


raw type        = Collection
unknown type    = Collection<?>
Generic type    = Collection<E>


So raw types are very much like wildcard types,
but they are not typechecked as stringently.
This is a deliberate design decision, to allow generics to interoperate with pre-existing legacy code.

Calling legacy code from generic code is inherently dangerous; once you mix generic code with non-generic legacy code, all the safety guarantees that the generic type system usually provides are void. However, you are still better off than you were without using generics at all. At least you know the code on your end is consistent


/******************************************************************************/
if (cs instanceof Collection<String>) { ... }
    --> runtime error
    Collection<?>
    ( or )
    Collection
    is ok
/******************************************************************************/
```


## java -version ==> 1.7 onwards angle brackets work

```java
i.e.

instead of
    List<String> list = new ArrayList<String>(); // used till 1.6

we can use,
    List<String> list = new ArrayList<>(); // diamonds!
```java


## Introduction

```java
List myIntList = new LinkedList(); // 1
myIntList.add(new Integer(0)); // 2
Integer x = (Integer) myIntList.iterator().next(); // 3

* Casting check(Integer) is done at runtime only.
* <Integer> generic check at compile time.

```java

## String equals comparison
```java
List <String> l1 = new ArrayList<String>();
List<Integer> l2 = new ArrayList<Integer>();
System.out.println(l1.getClass() == l2.getClass()); // true, type erasure and as well since string concat

public class Sample {
    public static void main(String[] args) {
        String s1 = "abcd";
        String s2 = new StringBuilder().append("ab").append("cd").toString();
        String s3 = s2;
        System.out.println(s1==s2); // false
        System.out.println(s2==s3); // true
        System.out.println(s1==s3);  // false
        s3 = "abcd";
        System.out.println(s1==s2); // false
        System.out.println(s2==s3); // false
        System.out.println(s1==s3); // true

        String a1 = "abcd";
        String a2 = "ab" + "cd";
        System.out.println(a1==a2); // true
    }
}

    Object[] objArr = new Object[3];
    String[] strArr = new String[3];

    objArr = strArr; // ok it works


    but,

    list to list not allowed
    i.e. List<String> to List<Object>

    List<String> list = new ArrayList<>();
    list.add("one");
    list.add("two");
    list.add("three");
    List<Object> lll = (List<Object>) list; // NOT allowed.
```


## some more ex
```java
import java.util.ArrayList;
import java.util.Collection;
import java.util.List;

public class ObjArr {
    static void fromArrayToCollection(Object[] a, Collection<Object> c) {
        for (Object o : a) {
            c.add(o); // compile-time error
        }
    }

    public static void main(String[] args) {

        List<String> list = new ArrayList<>();
        list.add("one");
        list.add("two");
        list.add("three");

        final String[] strsOrg = new String[3];
        for (String string : strsOrg) {
            System.out.println(">>>>" + string);
        }

        final String[] strs = list.toArray(strsOrg);

//>>>>>>>>>>>>
// list.toArray(strsOrg);
// or
// final String[] strs = list.toArray(strsOrg);
// both are same, see the last 2 lines of this pgm.


//final String[] obArr = (String[])list.toArray();
// No compile time error.
// Only at runtime error.(classcastexception)


        for (String string : strs) {
            System.out.println(string);
        }

        for (String string : strsOrg) {
            System.out.println(">>>>xxxxxxx=" + string);
        }


        System.out.println(strs); // [Ljava.lang.String;@9f5011
        System.out.println(strsOrg); // [Ljava.lang.String;@9f5011
    }

}
```


## Geenric methods:
```java
Objects are promoted to the  most specific type argument

static <T> void fromArrayToCollection(T[] a, Collection<T> c) {
    for (T o : a) {
        c.add(o); // Correct
    }
}


* Generic methods are used ONLY when dependency between type argument and
  return types exist.
```

## Using legacy code in generic code:
```java
@SuppressWarnings("unchecked")
@SuppressWarnings("unused")
@SuppressWarnings("rawtypes")
```

## @SuppressWarnings("unchecked")
```java

legacy code with raw types.

if method1(Collection) --> defined as argument of raw type.

    call from client code with method1(Collection<Part>) --> no warning,

but if there is a method with
    return raw type

    Collection method2()

from client code,

    Collection<Part> coll = method2();

    throws unchecked warning
```

## @SuppressWarnings("unused")
```java
we can add @SuppressWarnings("unused") or @SuppressWarnings("unchecked")
to the variable level , or as well to the method level.

if @SuppressWarnings is defined at var and method level,
    method level takes priority and var level shows warning as unnecessary.

 raw types are very much like wildcard types, but they are not typechecked as stringently
```

## Error or warning ????
```java
     1  public String loophole(Integer x) {
     2      List<String> ys = new LinkedList<String>();
     3      List xs = ys;
     4      xs.add(x);
     5      return ys.iterator().next();
     6  }
```

## VVI: (Very Very Important)
```java

    * Compile-time unchecked warning at line 4
    * Error at runtime in line 5

     1  public String loophole(Integer x) {
     2      List ys = new LinkedList;
     3      List xs = ys;
     4      xs.add(x);
     5      return(String) ys.iterator().next(); // run time error
     6  }
```
## Type variables
```java
//******************************************************************************
// Type variables don't exist at run time
//******************************************************************************
public class BadCast {
    @SuppressWarnings("unchecked")
    static <T> T badCast(T t, Object o) {
        return (T) o;
    }


    public static void main(String[] args) {
        System.out.println(badCast(112, "ob"));
    }
}

/******************************************************************************/
// Type variables don't exist at run time
/******************************************************************************/

        // Not really allowed.
        List<String>[] lsa = new List<String>[10];
            // but this is allowed.
            List<String>[] lsa = new List[10];

        Object o = lsa;
        Object[] oa = (Object[]) o;
        List<Integer> li = new ArrayList<Integer>();
        li.add(new Integer(3));
        // Unsound, but passes run time store check
        oa[1] = li;

        // Run-time error: ClassCastException.
        String s = lsa[1].get(0);


     ---- one more variation
        // OK, array of unbounded wildcard type.
        List<?>[] lsa = new List<?>[10];

        // Run time error, but cast is explicit.
        String s = (String) lsa[1].get(0);
```

## Class Literals as Runtime-Type Tokens
```java

Class .newInstance --> before generics ,,, returns Object
factory  object needed,

refer "C:\dhans\doc\java\generics\literals.html"

Now Class<T> .newInstance --> returns T ( the type parameter, which is very precious)
define the method as
    method1(Class<T> c)
        T item = c.newInstance();

    call as
        method1(Sample.class)
```

## More fun with wildcards:
```java
public static <T> T writeAll(Collection<T> coll, Sink<T> snk)

Sink<Object> s;
Collection<String> cs;
String str = writeAll(cs, s); // Illegal call.

        ---------------------
writeAll(Collection<T> coll, T[] snk)

Object[] ob;
Collection<String> cs;
String str = writeAll(cs, ob); // NOT OK

------------

        String[] strArr;

        Object[] objArr = { "one", "two" };

        strArr = (String[])objArr;
        // runtime exception: [Ljava.lang.Object; cannot be cast to [Ljava.lang.String;

        for (String str : strArr) {
            System.out.println(str);
        }
```

## Wildcard Capture
```java

Set<?> unknownSet = new HashSet<String>();
Set<?> s = Collections.unmodifiableSet(unknownSet); // This works! Why?

wildcard(unknownset) is captured as "s" // i think so,! it is named as
                                        // wildcard capture.

```

```java

covariant returns
-----------------

generics uses:
--------------

* strong type checking
* eliminate casts.
* generic algorithms.
```java

## TODO
```java
autoboxing
calendar class
cron jobs
prepare an example for bultiple bounds.
code coverage tools
```

## "unchecked" warning:
```java
assign raw type to parameterized type

Box rawBox = new Box();           // rawBox is a raw type of Box<T>
Box<Integer> intBox = rawBox;     // warning: unchecked conversion
```

## detail info about unchecked warning:
```java
Compile with -Xlint:unchecked


    disable unchecked warning -->
    -Xlint:-unchecked

    (or)

    @SuppressWarnings("unchecked")




On eclipse workspaces setting...
    "Java"
        Compiler
            Error/Warnings

    see the settings on warnings/error for "real use"
```

## Wildcard error
```java
package genmethods;

import java.util.ArrayList;
import java.util.List;

public class WildcardError {

    void foo(List<?> i) {
        //i.set(0, i.get(0)); --> compile error
        fooHelper(i); --> now NO error., explicit a compiler-akku sollu...
    }

    private <T> void fooHelper(List<T> list) {
                                         //--> T is CAP#1, the capture variable
        list.set(0, list.get(0));
    }
}
```java

## java generics is NOT only restricted to collections, but also to
```java
java.lang.reference
    --> Weak , Soft reference

java.util.concurrent.Callable
    --> call method

java.lang.Class
```

## type safety means
```java
    * compile with out error and warnings.
    * no ClassCastException at runtime.
```

## page 21
```java
wildcard capture
can not delare an exception with type parameter.
because of "type erasure".
i.e. type will be erased at runtime.

// sample:
--------------------------------------------------------------------------------

class GenericException<T> extends RuntimeException {
  // compilation error
  //    The generic class GenericException<T> may not subclass java.lang.Throwable

    /**
     *
     */
    private static final long serialVersionUID = 1L;

}

class SomeClass {

    public void method1() throws GenericException<String>, GenericException<Integer>  {
        // compilation error
        // Cannot use the parameterized type GenericException<String> either in catch block or throws clause
        // Cannot use the parameterized type GenericException<Integer> either in catch block or throws clause

    }

}

public class GenericExceptionSample {

    public static void main(String[] args) {
        final SomeClass someClass = new SomeClass();
        try {
            someClass.method1();
        } catch (GenericException<String> ge1) {
            // compilation error
            // Cannot use the parameterized type GenericException<String> either in catch block or throws clause

        } catch (GenericException<Integer> ge2) {
            // compilation error
            // Cannot use the parameterized type GenericException<Integer> either in catch block or throws clause

        }

    }
}

--------------------------------------------------------------------------------
```

## why SupressWarnings NOT allowed here ...... at the var. declaration level.
```java
i am not sure
```

## Mix Generics
```java
import java.util.ArrayList;
import java.util.List;

public class ParamNonParamMix {

    @SuppressWarnings("unchecked")
    public static void main(String[] args) {

        List<String> list1 = new ArrayList<>();

        @SuppressWarnings("rawtypes")
        List list2 = new ArrayList();

//why SupressWarnings NOT allowed here ...... at the var. declaration level.
//i am not sure
// why it has to be said at the main method level.???
        //@SuppressWarnings("unchecked")
        list1 = list2;
    }
}
```