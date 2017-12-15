---
layout: post
title:  "Effective Java - Part 1!"
date:   2016-03-13 9:30:46 +0900
categories: Effective Java, Part1
---

# Creating and Destroying Objects

## Item 1: Consider static factory methods instead of constructors
// NOT same as Factory Method pattern from Design Patterns [Gamma95, p. 107]  
// static factory method #(NE) Factory method pattern  
// no equivalent design pattern ( it is not a design pattern)  
```java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE;
}
```

// Advantages:
1. Name of method ( compared to constructors)  
2. Not required to create New object (Flyweight pattern)  
           == operator instead of the equals(Object) method, which may  
            result in improved performance  
3. return an object of any subtype (unlike constructors).  
            ex 1: Collections.static methods  
            ex 2: EnumSet  
                    64 or fewer ( RegularEnumSet)  
                    65 or more (JumboEnumSet)  
4. reduce verbosity ( by type inference)  

```java
    Map<String, List<String>> m =
        new HashMap<String, List<String>>();

    public static <K, V> HashMap<K, V> newInstance() {
        return new HashMap<K, V>();
    }

    Map<String, List<String>> m = HashMap.newInstance();

    // java 7 :
        // diamond operator
        Map<String, List<String>> m = new HashMap<>();

```

// Disadvantages:  
1. Without public / protected constructors, classes can NOT be subclassed.
Blessing in disguise : enforce composition instead of inheritance.  
2. naming convention required.
    * valueOf
    * of (concise alternative of valueOf, used in EnumSet)
    * getInstance
    * newInstance
    * getType
    * newType

1) can have names ( static factory method, NOT same as Factory Method pattern from Gamma)


            Factory method pattern:
            -----------------------

            public static Boolean valueOf(boolean b) {
                return b ? Boolean.TRUE : Boolean.FALSE;
            }
            ---
            final Boolean bool1 = new Boolean(true);
            final Boolean bool2 = new Boolean(true);

            System.out.println(bool1 == bool2); // false ( !!!! )

            System.out.println(bool1.hashCode()); // 1231
            System.out.println(bool2.hashCode()); // 1231

            System.out.println(System.identityHashCode(bool1)); // 9623250
            System.out.println(System.identityHashCode(bool2)); // 5602686
            --
            Boolean.hashCode()
                public int hashCode() {
                   return value ? 1231 : 1237;
                }
            }


            Abstract factory :
            ------------------
            use of composition

            public class LimitCalc {
                private Account ac;

                public LimitCalc(Account ac) {
                    this.ac = ac;
                }

                public BigInteger getBal() {
                    ac.getBal();
                }
            }

            class SAAccount {
                public BigInteger getBal() {
                    //....
                }
            }

            class CurrAccount {
                public BigInteger getBal() {
                    //...
                }
            }


            Factory pattern:
            ----------------
            use of inheritance:

---

## Item 2: Consider a builder when faced with many constructor parameters

* avoid telescoping constructor pattern
* use **builder** pattern

```java
package effjava;

//@formatter:off
// Sample Usage : java SampleBuilderPattern 2 3 
//@formatter:on
public class SampleBuilderPattern {

	public static void main(String[] args) {
		if (args.length < 2) {
			throw new IllegalArgumentException(
					"required and optional parameters missing. Sample Usage: java SampleBuilderPattern 2 3 ");
		}

		final int REQ_COUNT = Integer.valueOf(args[0]);
		final int OPT_COUNT = Integer.valueOf(args[1]);

		int tempReqCount = REQ_COUNT;
		int tempOptCount = OPT_COUNT;
//@formatter:off
// public class SampleBuilderBean { 
//@formatter:on
		System.out.println("public class SampleBuilderBean {");
		System.out.println();

//@formatter:off
//	    // Required
//	    private final reqType1 reqProperty1;
//	    private final reqType2 reqProperty2;
//
//	    // Optional
//	    private final opType1 opProperty1;
//	    private final opType2 opProperty2;
//	    private final opType3 opProperty3;
//	    
//@formatter:on
		System.out.println("    // Required");
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.println("    private final reqType" + i + " reqProperty"
					+ i + ";");
		}
		System.out.println();
		System.out.println("    // Optional");
		for (int i = 1; i <= tempOptCount; i++) {
			System.out.println("    private final opType" + i + " opProperty"
					+ i + ";");
		}
//@formatter:off
// public static class Builder {
//@formatter:on
		System.out.println();
		System.out.println("    public static class Builder {");
		System.out.println();

//@formatter:off
//        // Required parameters
//        private final reqType1 reqProperty1;
//        private final reqType2 reqProperty2;
//
//        // Optional parameters - initialized to default values
//        // TODO : initialize to default
//        private opType1 opProperty1 = ;
//        private opType2 opProperty2 = ;
//        private opType3 opProperty3 = ;
//   
//@formatter:on
		tempReqCount = REQ_COUNT;
		tempOptCount = OPT_COUNT;
		System.out.println("        // Required parameters");
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.println("        private final reqType" + i
					+ " reqProperty" + i + ";");
		}
		System.out.println();
		System.out
				.println("        // Optional parameters - initialized to default values");
		System.out.println("        // TODO : initialize to default");
		for (int i = 1; i <= tempOptCount; i++) {
			System.out.println("        private opType" + i + " opProperty" + i
					+ " = " + ";");
		}
		System.out.println();

//@formatter:off
// public Builder(reqType1 reqProperty1, reqType2 reqProperty2) {
//@formatter:on

		System.out.print("        public Builder(");
		tempReqCount = REQ_COUNT;
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.print("reqType" + i + " reqProperty" + i);
			if (i < tempReqCount) {
				System.out.print(", ");
			}
		}
		System.out.println(") {");
//@formatter:off
//    this.reqProperty1 = reqProperty1;
//    this.reqProperty2 = reqProperty2;
//  }
//@formatter:on
		tempReqCount = REQ_COUNT;
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.println("            this.reqProperty" + i + " = "
					+ "reqProperty" + i + ";");
		}
		System.out.println("        }");
//@formatter:off
//    public Builder opProperty1(opType1 val) {
//        opProperty1 = val;
//        return this;
//    }
//    public Builder opProperty2(opType2 val) {
//        opProperty2 = val;
//        return this;
//    }
//    public Builder opProperty3(opType3 val) {
//        opProperty3 = val;
//        return this;
//    }
//@formatter:on
		tempOptCount = OPT_COUNT;
		for (int i = 1; i <= tempOptCount; i++) {
			System.out.println("        public Builder opProperty" + i
					+ "(opType" + i + " val) {");
			System.out.println("            " + "opProperty" + i + " = "
					+ "val;");
			System.out.println("            " + "return this;");
			System.out.println("        }");
		}
//@formatter:off
//    public SampleBuilderBean build() {
//        return new SampleBuilderBean(this);
//    }
// }		
//@formatter:on
		System.out.println("        public SampleBuilderBean build() {");
		System.out.println("            return new SampleBuilderBean(this);");
		System.out.println("        }");
		System.out.println("    }");

		System.out.println();
//@formatter:off
//    private SampleBuilderBean(Builder builder) {
//        reqProperty1 = builder.reqProperty1;
//        reqProperty2 = builder.reqProperty2;
//        opProperty1 = builder.opProperty1;
//        opProperty2 = builder.opProperty2;
//        opProperty3 = builder.opProperty3;
//    }
//@formatter:on
		System.out.println("    private SampleBuilderBean(Builder builder) {");
		tempReqCount = REQ_COUNT;
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.println("        reqProperty" + i + " = "
					+ "builder.reqProperty" + i + ";");
		}

		tempOptCount = OPT_COUNT;
		for (int i = 1; i <= tempOptCount; i++) {
			System.out.println("        opProperty" + i + " = "
					+ "builder.opProperty" + i + ";");
		}
		System.out.println("    }");
//@formatter:off
//    public reqType1 reqProperty1() {
//        return reqProperty1;
//    }
//@formatter:on
		System.out.println();
		tempReqCount = REQ_COUNT;
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.println("    public reqType" + i + " reqProperty" + i
					+ "() {");
			System.out.println("        return reqProperty" + i + ";");
			System.out.println("    }");
		}
		System.out.println();
		tempOptCount = OPT_COUNT;
		for (int i = 1; i <= tempOptCount; i++) {
			System.out.println("    public opType" + i + " opProperty" + i
					+ "() {");
			System.out.println("        return opProperty" + i + ";");
			System.out.println("    }");
		}

//@formatter:off
//	    public static void main(String[] args) {
//	        final SampleBuilderBean sbb = new SampleBuilderBean.Builder(reqactval1, reqactval2)
//	                                                           .opProperty1(opVal1)
//	                                                           .opProperty2(opVal2)
//	                                                           .opProperty3(opVal3)
//	                                                           .build();
//	    }
//@formatter:on
		System.out.println("    public static void main(String[] args) {");
		System.out
				.print("        final SampleBuilderBean sbb = new SampleBuilderBean.Builder(");

		tempReqCount = REQ_COUNT;
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.print("reqactval" + i);
			if (i < tempReqCount) {
				System.out.print(", ");
			}
		}
		System.out.println(")");

		tempOptCount = OPT_COUNT;
		for (int i = 1; i <= tempOptCount; i++) {
			System.out
					.println("                                                           .opProperty"
							+ i + "(opVal" + i + ")");
		}
		System.out
				.println("                                                           .build();");

		System.out.println("    }");

//@formatter:off
//	}
//@formatter:on
		System.out.println("}");
	}

//@formatter:off
/* Sample Output
public class SampleBuilderBean {

    // Required
    private final reqType1 reqProperty1;
    private final reqType2 reqProperty2;

    // Optional
    private final opType1 opProperty1;
    private final opType2 opProperty2;
    private final opType3 opProperty3;

    public static class Builder {

        // Required parameters
        private final reqType1 reqProperty1;
        private final reqType2 reqProperty2;

        // Optional parameters - initialized to default values
        // TODO : initialize to default
        private opType1 opProperty1 = ;
        private opType2 opProperty2 = ;
        private opType3 opProperty3 = ;

        public Builder(reqType1 reqProperty1, reqType2 reqProperty2) {
            this.reqProperty1 = reqProperty1;
            this.reqProperty2 = reqProperty2;
        }
        public Builder opProperty1(opType1 val) {
            opProperty1 = val;
            return this;
        }
        public Builder opProperty2(opType2 val) {
            opProperty2 = val;
            return this;
        }
        public Builder opProperty3(opType3 val) {
            opProperty3 = val;
            return this;
        }
        public SampleBuilderBean build() {
            return new SampleBuilderBean(this);
        }
    }

    private SampleBuilderBean(Builder builder) {
        reqProperty1 = builder.reqProperty1;
        reqProperty2 = builder.reqProperty2;
        opProperty1 = builder.opProperty1;
        opProperty2 = builder.opProperty2;
        opProperty3 = builder.opProperty3;
    }

    public reqType1 reqProperty1() {
        return reqProperty1;
    }
    public reqType2 reqProperty2() {
        return reqProperty2;
    }

    public opType1 opProperty1() {
        return opProperty1;
    }
    public opType2 opProperty2() {
        return opProperty2;
    }
    public opType3 opProperty3() {
        return opProperty3;
    }
    public static void main(String[] args) {
        final SampleBuilderBean sbb = new SampleBuilderBean.Builder(reqactval1, reqactval2)
                                                           .opProperty1(opVal1)
                                                           .opProperty2(opVal2)
                                                           .opProperty3(opVal3)
                                                           .build();
    }
}
 */
//@formatter:on	

}


```

---

## Item 3: Enforce the singleton property with a private constructor or an enum type

**Singleton with public final field**
```java

// Serializable Singleton
public class Singleton implements Serializable {
    private static final Singleton INSTANCE = new Singleton();
    private Singleton() { 
        //... 
        // TODO throw exception, on second instance creation
        // when using AccessibleObject.setAccssible
    }
    public static Singleton getInstance() { return INSTANCE; }

    public void leaveTheBuilding() { 
        //... 
    }
    
    // readResolve method to preserve singleton property
    private Object readResolve() {
        // Return the one true Elvis and let the garbage collector
        // take care of the Elvis impersonator.
        return INSTANCE;
    }

}

// TODO implement AccessibleObject.setAccessible
```

### Enum singleton - the preferred approach  

1. guarantee against multiple instantiation, 
2. even in the face of sophisticated serialization or 
3. reflection attacks

```java
// java 1.5 using enum for creating Singleton
public enum Singleton {
    INSTANCE;
    public void leaveTheBuilding() { ... }
}

```

---

## Item 4: Enforce non-instantiability with private constructor  

### utility class

**examples**

* java.lang.Math
* java.util.Arrays
* java.util.Collections

1. Make it abstract ...hmm no: since it can be subclassed and can be instantiated.
2. Make noninstantiable by including a private constructor

```java
// Noninstantiable utility class
public class UtilityClass {
    // Suppress default constructor for noninstantiability
    private UtilityClass() {
        throw new AssertionError();
    }
    // Remainder omitted
}
```

## Item 5: Avoid creating unnecessary objects.

    Reuse objects. use static blocks for init and then reuse objects.
    String s = new String("stringette"); // DON'T DO THIS!

```java
    class AvoidUnnecessaryObjectsCreationWithStatic {
        private static final Type1 varName1;
        private static final Type2 varName2;
        static {
            varName1 = ...
            varName2 = ...
        }

        public void checkMethod() {
            // using varName1, varName2 for calculation
        }
        // Also check in loops for creating objects
    }


    // also
    // prefer primitives to boxed primitives,
    // and watch out for unintentional autoboxing
```


## Item  6: Eliminate obsolete object references
```java
public Object pop() {
    if (size == 0)
        throw new EmptyStackException();

    Object result = elements[--size];

    elements[size] = null; // Eliminate obsolete reference

    return result;
}
```
WeakHashMap

LinkedHashMap.removeEldestEntry

HeapProfiler


    //TODO when pop out references
```java

how is the interface method access priviliges affect the implemeted methods.

please refer

package item2;

interface MyStackIntf {
  void push(Object ob);
  Object pop();
  int size();
}

package item2;

import java.util.Arrays;

public class MyStack implements MyStackIntf {
  
  private int initCapa = 10;
  private int size = 0;
  
  private Object[] obArr = new Object[initCapa];

  public void push(Object ob) {
    size();
    obArr[size++] = ob;
  }

  public Object pop() {
    size = size-1;
    Object popObject = obArr[size];
    obArr[size] = null;
    return popObject;
  }
  
  
  public int size() {
    if (size == initCapa) {
      System.out.println(">>>>>>resizing");
      initCapa = initCapa + 10;
      obArr = Arrays.copyOf(obArr, initCapa);
    }
    return size;
  }
  
/*
 
  size      initcapa   obArr[0...9] = null, null, ...null         size
  0       10                            0
  
 push1            obArr[0] = push1              1 
  1
  
 push2            obArr[1] = push2                  2
  2
  
   
 push9                      obArr[9] = push9              9
 
 push10        size()       obArr[10]= push10             10
               initCapa 
               20            
               
 push11                                     11
  
  
  
  
 push19           obArr[19]=push19              19
 
  
 push20     initCapa30  obArr[20]=push
      
  
  
 */
  
  
}



Note:
when running a loop, make sure only one variable is modified.
if multitple variables are modified and
both variable determines the loop execution condition,
your fundamental programming is (mostly) wrong
and will be complex to debug even if it is working fine.


Eliminate obsolete object references:
    --> * how to do this ?
         limit the scope of the reference to the object, i.e. variable
         so that if the variable goes out of scope, it is eligible for gc.

         also be careful in the example, MyStack.java, the pop method has

         obArr[size] = null;

         eventhough we are reducing the size of the stack during stack,
         the object referred in the array still there.

         i.e. as per application-wise the stack the size, and we refer the size
         to loop stack contents.

         but system-wise the obArr still holding the reference to the object.



```

## Item 7: Avoid finalizers -- @ home
```java
1) Increase the odds of finalizers getting executed...
    But NOT guarantee it.

    System.gc()
            -->  Runtime.getRuntime().gc();

    System.runFinalization()
            --> Runtime.getRuntime().runFinalization();

2) The evil methods that guarantee

    System.runFinalizersOnExit()
            -->  Runtime.runFinalizersOnExit()

Add explicit close() methods:
    InputStream
    OutputStream
    java.sql.Connection

    java.util.Timer.cancel()

    java.awt
        Graphics.dispose()
        Window.dispose()

    Image.flush()

finally methods in
    FileInputStream
    FileOutputStream
    Timer
    Connection

finalizer chaining

@Override protected void finalize() {
    try {

    } finally {
        super.finalize();
    }
}
    --

finalizer guardian

public non final　class ( so that subclass not required to call super.finalize())

    public class MyNonFinal {
        private Object finalizerGuardian = new Object() {
            @Override protected void finalize() throws Throwable {
                // finalize outer foo.
            }
        }
    }
```

## Item 8: Obey general contract when overriding equals
```java
    why have to use
        Float.compare
        Double.compare

    when comparing float and double primitivies?


    Object methods.
        equals()
            write test cases for
                1) Reflexive x.equals(x)
                2) Symmetry x.equals(y) y.equals(x)
                3) Transiive x.equals(y)  y.equals(z)  x.equals(z)
                4) consistency, x.equals(y) multiple times, and also 1) to 3)
                        multiple times.
                5) x non null , then x.equals(null) --> false

        Subclassing value class violate symmetry
        Favour composition over inheritance.

        @Override public boolean equals(Object o) {
            if (!(o instanceof MyType)) {
                return false; // returns false also if o is null
            }
            MyType mt = (MyType) o;
            ...
        }

        sample from AbstractList

            public boolean equals(Object o) {
                if (o == this)
                    return true;
                if (!(o instanceof List))
                    return false;
                    ....

        // Float.compare
        // Double.compare
        // Arrays.equals(float[], float[]) ....hmmm
            have to use Float.compare inside Arrays.equals

        also for equals method check hash value and store it inside the field.

        field check equality inside equals method
        (field == o.field || (field != null && field.equals(o.field)))

    ------
    hashCode of double:

    @Override public int hashCode() {
        int result = 17 + hashDouble(re);
        result = 31 * result + hashDouble(im);
        return result;
    }
    private int hashDouble(double val) {
        long longBits = Double.doubleToLongBits(re);
        return (int) (longBits ^ (longBits >>> 32));
    }
    ------

        java.util.Random NOT overriding equals

    AbstractSet, AbstractList, AbstractMap --> override equals

    Set, List, Map --> NOT overriding equals


    // Should not allow calling equals...
    @Override public boolean equals(Object obj) {
        throw new AssertionError();
    }
```

## Item 12: Consider implementing Comparable
```java
        public interface Comparable<T> {
            int compareTo(T t);
        }
```
## Item 20: Prefer class hierarchies to tagged classes

## Item 21: Use function objects to represent strategies
```java
    why implement serializable in comparator.
    See the example: (page 105)
    class Host {
    private static class StrLenCmp
    implements Comparator<String>, Serializable {
```

## Item 53: Prefer interfaces to reflection

## Chapter 5: Generics

## Item 29: Consider typesafe heterogeneous containers

---------------------------

## Item 66: Synchronize access to shared mutable data
```
//////////////////////////////////////////////////

    Atomic ? yes / no ?
        Read / Write --> var
        => yes

    Atomic ? yes / no ?
        Read /write --> var of type "long" / "double"
        => no

    even then, updated values in one thread may NOT reflect in other thread.

    This is because of the "memory model".

    Synchronization "for" reliable communication

    hoisting ( optimization by VM)
        while(!done) {
            i++;
        }

        =>

        if(!done) {
            while(true) {
                i++;
            }
        }

    But put a System.out.println() inside the while loop, the program terminates!

    We can't rely on System.out.println, so use "synchronized" for writing and
    reading to a var.

    /////////////////////////////////////////////
    public class MyStopRequest {

        // Properly synchronized cooperative thread termination
        private static boolean stopRequested;

        private static synchronized void requestStop() {
            stopRequested = true;
        }

        private static boolean stopRequested() {
            return stopRequested;
        }

        public static void main(String[] args) throws InterruptedException {
            Thread backgroundThread = new Thread(new Runnable() {
                public void run() {
                    int i = 0;
                    while (!stopRequested())
                        i++;
                }
            });
            backgroundThread.start();
            TimeUnit.SECONDS.sleep(1);
            requestStop();
        }

    }
    \\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\

    Note: reading also requires "synchronized", so above example
    will NOT terminate


    volatile can be used instead of synchronized in the above method.

    volatile on boolean field is ok.
    it volatile var(i) is for int field. and i++(two step operation)
    may not reflect the correct value in other thread. => safety failure.

    AtomicLong ( how it provide atomicity )
    CAS ( compare and swap)

    http://winterbe.com/posts/2015/05/22/java8-concurrency-tutorial-atomic-concurrent-map-examples/

    write a simple example using AtomicInteger to prove it is atomic

    ////////////////////////////////////////////////////////////////////////////
    AtomicInteger atomicInt = new AtomicInteger(0);
    ExecutorService executor = Executors.newFixedThreadPool(2);
    IntStream.range(0, 1000)
        .forEach(i -> executor.submit(atomicInt::incrementAndGet));
    stop(executor); // ? what is stop here
    System.out.println(atomicInt.get());    // => 1000
    \\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\

    safe publication / effectively immutable
    Use
        1) volatile field
        2) final field
        3) field with normal locking
        4) put the field in concurrent collection ( brilliant idea ?! check it out)


    inter-thread communication VS mutual exclusion
    ( stopRequested boolean var) VS ( int++ )
```
## Item 67: Avoid excessive synchronization
```
////////////////////////////////////////

    Calling unknown methods from synchronized method cause problem...

    Observer pattern

    How to create deadlock when iterating through the list and remove item
    while iterating...


    alien method invocations out of the synchronized block, and create a copy.

    Or create CopyOnWriteArrayList

    StringBuffer vs StringBuilder

    lock splitting, lock striping, nonblocking concurrency control
```

## Item 70: Document Thread Safety:
```
////////////////////////////////

    Map<K,V> synchedMap = Collections.synchronizedMap(somemap);

    // NOT required. because , synchedMap.keySet() already takes care.
    synchronized(synchedMap) {
        Set<K> synchedSet = synchedMap.keySet();
    }


    Set<K> synchedSet = synchedMap.keySet();
   /// NO, have to lock on backing object
   /// synchronized(synchedSet) {
       synchronized(synchedMap) {

            Iterator<K> it = synchedSet.iterator();
            while(it.hasNext()) {
                ...
        }
   }
```
## Item 71: Use Lazy initialization judiciously
```
////////////////////////////////////////////
    Lazy initialization holder class idiom /
    On demand holder class idiom
    For static fields


    private static class FieldHolder {
        static final FieldType field = computeFieldValue();
    }

    static FieldType getField() { return FieldHolder.field ;}

    ~~~~~~~~~~~~

    // Double-check idiom for lazy initialization of instance fields

    private volatile FieldType field;

    FieldType getField() {
        FieldType result = field;
        if (result == null) { // First check (no locking).
                              // that is why "volatile" required.
                              // imp. prepare a test case for this.
            synchronized(this) {
                result = field;
                if (result == null) // Second check (with locking)
                field = result = computeFieldValue();
            }
        }
        return result;
    }


    // Double check idiom tolerating repeated initialization.
    // becomes --> Single check idiom

    private volatile FieldType field;
    // if FieldType is NOT long/double , can remove volatile.
    // --> racy single check idiom.

    FieldType getField() {
        FieldType result = field;
        if (result == null) {
                field = result = computeFieldValue();
            }
        }
        return result;
    }
```
## Item 72: Don't depend on thread scheduler.
```
///////////////////////////////////////////

    how is AtomicInteger work and make sure not overlapping between threads.

    latch --> terminal state --> gate opens and --> thread pass

    runnable threads <= no. of processors

    avoid;
        1) using thread priority ( not portable)
        2) Thread.yield ( no testable semantics )

    Threads SHOULD NOT busy wait
        // repeatedly checking shared object
        // to check something happen
        while (true) {
            synchronized(this) {
                if (count == 0) return;
            }
        }

    Use Thread.sleep(1) instead of Thread.yield for concurrency testing

    Thread.sleep(0) --> return immediately. dont use it.


    // write a blog about CountDownLatch


    Don't depend on thread scheduler.

    * Don't use
        Thread.yield
            or
        Thread.sleep(0)

        for concurrency testing

    * Use
        Thread.sleep(1)
            for concurrency testing
```
## Item 73: Avoid thread groups
```
/////////////////////////////

Avoid thread groups:
            originally intended for applet.

Favour
    Thread.setUncaughtExceptionHandler
    instead of
    ThreadGroup.uncaughtException (but prior to 1.5 this was the ONLY way.)

    ThreadPoolExecutors


    wait() must be called from synchronized block.
    Otherwise IllegalMonitorStateException is thrown.

    Use
    Thread.setUncaughtExceptionHandler ( now use)
    instead of ThreadGroup.uncaughtException ( prior to 1.5 only way)

    Most of methods suspend, resume are deprecated.
    So threadgroup not in use.

    Use Executors

Finding  the  right  balance  of  sequentiality and  asynchrony  is  often  a  characteristic  of
efficient people  and the same is true of programs.
```
--------------------------------------------------------------------------------

## Item 74: Implement Serializable judiciously
```
--------------------------------------------------------------------------------
    Dont accept the default searialized form
    Why?

    Changing the internal representation of object via
    ObjectInputStream.readFields()
    ObjectOutputStream.putFields()

    what is the purpose of serialVersionUID?

    what is readObjectNoData() ?
        1) serialize object
        2) Change the super class of the serialized class
        3) deserialized object with the new updated class
        4) readObjectNoData() is called back
    cover a corner case involving the addition of a serializable superclass to an
    existing serializable class.
```
--------------------------------------------------------------------------------

## Item 78: Consider serialization proxies instead of serialized instances
```
--------------------------------------------------------------------------------
Serialization proxies
---------------------

Serialized instances: ---> NO

Serialization proxies --> YES


Implementing Serializable : following are the problems:
    1) Bugs
    2) Security problems
    3) causes instances to be created using an extralinguistic mechanism in
        place of ordinary constructors

Avoid the above risks:
----------------------
    Serialization Proxy Pattern

/******************************************************************************/

    private static class SerializationProxy implements Serializable {

        private static final long serialVersionUID = 111222333444555666L;

        private final Date start;
        private final Date end;

        SerializationProxy(Period p) {
            this.start = p.start;
            this.end = p.end;
        }
    }


    private Object writeReplace() {
        return new SerializationProxy(this);
    }

```
----

```
All the above discussed detail is the item 
# 1) major cost of implementing Serializable.

i.e. cost #1) Decrease the flexibility to change class implementation.

cost # 2) DeSerialization : hidden constructor => invariant corruption and illegal access.

cost # 3) testing burden associcated with each new version.
  test must ensure
    a) binary compatibility 
    b) semantic compatibility 


Rule of thumb for designing whether to implement Serializable or not.

  1) Value classes -> implements Serializable
    ex: Date, BigInteger, and most collection classes.
  2) Acitive entities ( classes representing) rarely implements Serializable.

    Dont accept the default searialized form
    Why?

    Changing the internal representation of object via
    ObjectInputStream.readFields()
    ObjectOutputStream.putFields()

    what is the purpose of serialVersionUID?

    what is readObjectNoData() ?
        1) serialize object
        2) Change the super class of the serialized class
        3) deserialized object with the new updated class
        4) readObjectNoData() is called back
    cover a corner case involving the addition of a serializable superclass to an
    existing serializable class.
   
------------------------

Inner class should not implement Serializable
  A static member class can, however, implement Serializable
```

/******************************************************************************/

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/