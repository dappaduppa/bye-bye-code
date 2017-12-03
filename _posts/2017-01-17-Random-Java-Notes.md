---
layout: post
title:  "Random Java Notes"
date:   2017-01-17 09:28:46 +0900
categories: Java, Random
---

assert:
------
	-ea 
		or 
	-enableassertions 

	--> vm switch to enable assertion ( by default assertion is disabled).  
    java -ea MySample.java

guava:
---
	Preconditions 
	advantage: message can take parameters.
	ex: Preconditions.checkArgument
    (start < end, "Start (%s) must be smaller than end (%s)", start, end);  

Spring:
---
	Assert
	Assert.isTrue
    (start < end, "Start (" + start + ") must be smaller than end (" + end + ")");
	disadv: but message can not take parameters, 
    has to construct the entire msg as a string.
		
## "Null sucks." -Doug Lea

"I call it my billion-dollar mistake." - Sir C. A. R. Hoare, on his invention of the null reference

Time elapsed in Java program:
------------
```java
System.nanoTime()
//uses some fixed arbitrary for time elapsed , 
//Not System ( "wall" ) clock.
			
System.currentTimeMillis()            
The purpose of nanoTime is to measure elapsed time, 
and the purpose of currentTimeMillis is to measure wall-clock time.
( may vary since bg corrects this wall clock time)
```

## javadoc

```java
Looking like code :

 
The <code></code> tag is not referencing a specific parameter. 
It is formatting the word "String" into "code looking" text. 

you can use {@code foo}. 
 
This is especially good to know when you refer to a generic type 
such as {@code Iterator<String>} 
-- sure looks nicer than <code>Iterator&lt;String&gt;</code>, doesn't it!

Link:

{@linkplain java.nio.charset.Charset chset}	--> 
	link/anchor name will be "chset"
		
How to add reference to a method parameter in javadoc?
	--> Not possible.
```

## equals

```java
"test" == "test" // --> true 

// ... but you should really just call Objects.equals()
Objects.equals("test", new String("test")) // --> true
Objects.equals(null, "test") // --> false


// Code of Object.toString()
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}


[Ljava.lang.String means:
" [ - an single-dimensional array (as opposed to [[ or [[[ etc.)
" L - the array contains a class or interface
" java.lang.String - the type of objects in the array

```

				
**what is the issue with the code:**
```java
import javax.script.ScriptEngineManager;
import javax.script.ScriptEngine;
import javax.script.ScriptException;

public class Test {
  public static void main(String[] args) throws ScriptException {
    ScriptEngineManager mgr = new ScriptEngineManager();
    ScriptEngine engine = mgr.getEngineByName("JavaScript");
    String foo = "40+2";
    System.out.println(engine.eval(foo));
    } 
}

	new javax.script.ScriptEngineManager().getEngineByName("JavaScri  pt") .eval("var f = new java.io.FileWriter('hello.txt'); f.write('UNLIMITED POWER!'); f.close();"); -- will write a file via JavaScript into (by default) the program's current directory
	
	Also be aware of floating-point math. 0.1+0.2 will evaluate to 0.30000000000000004
	
	(float) (0.2+0.1) => 0.3
```
	
**String.join**
```java	
java8:	String.join(delimiter, String array)
go   :  strings.join([]String{"a", "b"}, "-")


java8: String.join("", Collections.nCopies(5, "a"));
	   String.format("%0" + n + "d", 0).replace("0",s);
go   : strings.Repeat("a", 5)
```

---------------


**please study about nio Charset class**

https://stackoverflow.com/questions/tagged/java+string?page=1&sort=frequent&pagesize=50
	
https://stackoverflow.com/questions/309424/read-convert-an-inputstream-to-a-string


	StreamSupport.stream(iter.spliterator(), false);
	
	Why is "final" not allowed in Java 8 interface methods? 
	
Well grounded Java developer book
----------------------------
	LinkedTransferQueue
	fork/join fwk
	ConcurrentHashMap
	Atomic classes
	
	
JMM ( Java Memory Model )
	Happens-Before
	Synchronizes-with
	
	
RecursiveAction extends ForkJoinTask<Void>

* ForkJoinTask scheduled in ForkJoinPool


Introduction to Algorithms
-------------------------

The Algorithm Design Manual, second edition
---------------------------


ReentrantLock ( by default avoid deadlock??)
ReentrantReadWriteLock


lock.tryLock(10, TimeUnit.SECONDS);

CountDownLatch

Multiple countdownlatch can be written with One CyclicBarrier


ConcurrentHashMap equivalent -> CopyOnWriteArrayList

BlockingQueue
	put
	take
	
		LinkedBlockingQueue
		ArrayBlockingQueue
		

BlockingQueue<WorkUnit<MyAwesomeClass>>

	public class WorkUnit<T> {
		private final T workUnit;
		public T getWork(){ return workUnit; }
		public WorkUnit(T workUnit_) {
		workUnit = workUnit_;
		}
	}



// plan practical example for deadlock, race condn., etc


TransferQueue




https://manning-content.s3.amazonaws.com/download/e/15b9513-9763-41e7-9178-5cded4d02996/TWGJD_sample_ch04.pdf

https://translate.googleusercontent.com/translate_c?act=url&depth=1&hl=ja&ie=UTF8&prev=_t&rurl=translate.google.co.in&sl=ja&sp=nmt4&tl=en&u=https://stackoverflow.com/questions/10202768/is-java-concurrency-in-practice-still-valid/10214606&usg=ALkJrhjNRAMkeT_3oBOVRs9ZqruHssW7hw#10214606

https://translate.googleusercontent.com/translate_c?act=url&depth=1&hl=ja&ie=UTF8&prev=_t&rurl=translate.google.co.in&sl=ja&sp=nmt4&tl=en&u=https://stackoverflow.com/users/3553087/brian-goetz&usg=ALkJrhjxyuU-S3wzFfgDpc78mK7KjAq6uQ



------------

javaworld

	System.in.read() 
	
		ch <= '9' ch >= '0' 
		
		
write wordcount using regex in java

---------
anonymous inner class cant be generic ??? why?
cant understand

http://viewsourcecode.org/snaptoken/kilo/02.enteringRawMode.html

please also study http://www.joda.org/joda-time/userguide.html


jvisualvm consists of ....

    jconsole
    jstat
    jinfo
    jstack
    jmap

Dont directly copy the no. and expect the same behaviour.
        //Thread.sleep(2000); --> 2 secs
        TimeUnit.SECONDS.sleep(2000); --> 2000 secs

```java
//TODO
jstat ....

jstat -compiler <pid>

jstat -compiler 3020
Compiled Failed Invalid   Time   FailedType FailedMethod
     123      0       0     0.12          0
    -->

jstat -printcompilation <pid> <freq in millis>

jstat -printcompilation 3020 10
Compiled  Size  Type Method
     124    630    1 ds/NoVisibility$ReaderThread run
     124    630    1 ds/NoVisibility$ReaderThread run
     124    630    1 ds/NoVisibility$ReaderThread run

//TODO
yourkit  -- java profiling

browserstack -- cross browser testing

jfrog artifactory --> hosting spring projects

MaxPermSize --> MaxMetaspaceSize

ClassMetaSpaceSize --> default 2Mb

```


//TODO: moores law

Chapter 16. The Java Memory Model

And  as  processor  manufacturers  transition  to
multicore processors, largely because clock rates are getting
harder to increase economically, hardware parallelism will
only increase.

---

BigInteger usage and BigDecimal usage

How wrapper classes are NOT suitable for callback frameworks.

B really an A?
    NO...then....B should not extend A.



## Functional Interfaces:

```java
java.util.function

1) Predicate test()
2) BiFunction   apply()
3) BiConsumer 2 inputs , but dont return....
4) BinaryOperator apply().... same as BiFunction, but also same types for input and output
5) BiPredicate  test() two arguments
6) Consumer ....accept as array or list and iterate...
    please refer C:\dhans\src\java\src\MyRepeatingAnno.java

SafeVarargs
    since there is no new T[] in java possible,
    it will be new Object[] for the varargs....
    so the compiler throws the warning .. SafeVarargs.

    please refer the javadoc....you will understand ...
    inside the method, there is a possiblity of ClassCastException happen
    because of the developer.

```

## Java SE 8

```java
As of the Java SE 8 release, annotations can also be applied to the use of types. 
Here are some examples:

Class instance creation expression:
    new @Interned MyObject();
Type cast:
    myString = (@NonNull String) str;
implements clause:
    class UnmodifiableList<T> implements
        @Readonly List<@Readonly T> { ... }
Thrown exception declaration:
    void monitorTemperature() throws
        @Critical TemperatureException { ... }
This form of annotation is called a type annotation. 
For more information, see Type Annotations and Pluggable Type Systems.
```


## Queue extends Collection

```java
additional methods:
    does not throw exception

FIFO

elements added at the tail of the queue.

do not use (avoid use) null ( eventhough queue allows it)

PriorityQueue --> sort

java.util.Queue ... not bounded

java.util.concurrent.queue ---> bounded


bound means --> restrict the no. of elements it hold.

java.util.concurrent.BlockingQueue --> wait for something like space ...etc
In general this BlokingQueue kind not avalible in java.util.Queue

```

## checker framework
    https://checkerframework.org/tutorial/
    for ex. finding NPE


## artificial neuron 
    (the perceptron and the sigmoid neuron),
    
## stochastic gradient descent
    std algo.

```java
jconv
jcoco
eclemma
bulk operations with stream api:

date issues in java and mysql
how is executor service is better than threads.
data visualization techniques...
Go wiki links
stackoverflow user top listing and see their questions and answers.
Data Analysis Using SQL and Excel BOOK
PANDAS lib in python 
boltclock
kilo
```

## murmurhash
http://code.google.com/p/smhasher/


```java
For LinkedList<E>
?get(int index) is O(n/4) average
?add(E element) is O(1)
?add(int index, E element) is O(n/4) average
      but O(1) when index = 0 <--- main benefit of LinkedList<E>
?remove(int index) is O(n/4) average
?Iterator.remove() is O(1) <--- main benefit of LinkedList<E>
?ListIterator.add(E element) is O(1) <--- main benefit of LinkedList<E>
Note: O(n/4) is average, O(1) best case (e.g. index = 0), O(n/2) worst case (middle of list)
For ArrayList<E>
?get(int index) is O(1) <--- main benefit of ArrayList<E>
?add(E element) is O(1) amortized, but O(n) worst-case since the array must be resized and copied
?add(int index, E element) is O(n/2) average
?remove(int index) is O(n/2) average
?Iterator.remove() is O(n/2) average
?ListIterator.add(E element) is O(n/2) average

java collection tough interview questions
---------------------------------------------
java.util.concurrent.BlockingQueue
Set, List , Map
ConcurrentModificationException
CopyOnWriteArrayList
Hashtable --> ConcurrentHashMap
		putIfAbsent
ArrayStoreException
NavigableMap
HashSet depends on HashMap
Collections.sort
Synchronized Collection and Concurrent Collection
--------------------
ActionListener.actionPerformed
invokeLater 
invokeAndWait
javax.swing.SwingUtilities.isEventDispatchThread
	--> java.awt.EventQueue.isDispatchThread()
Thread Interference

For simplicity, consider the data in the range 0 to 9. 
Input data: 1, 4, 1, 2, 7, 5, 2
  1) Take a count array to store the count of each unique object.
  Index:     0  1  2  3  4  5  6  7  8  9
  Count:     0  2  2  0   1  1  0  1  0  0
  2) Modify the count array such that each element at each index 
  stores the sum of previous counts. 
  Index:     0  1  2  3  4  5  6  7  8  9
  Count:     0  2  4  4  5  6  6  7  7  7
The modified count array indicates the position of each object in 
the output sequence.
 
  3) Output each object from the input sequence followed by 
  decreasing its count by 1.
  Process the input data: 1, 4, 1, 2, 7, 5, 2. Position of 1 is 2.
  Put data 1 at index 2 in output. Decrease count by 1 to place 
  next data 1 at an index 1 smaller than this index.
  Index:     0  1  2  3  4  5  6  7  8  9
                11   2 2   4 5   7
                
                
-------
radix sort
		--> (calls) counting sort.
		
bucket sort	 --> (requires)
			linked lists, dynamic arrays
			
	but "counting sort" stores single number
couning sort NE comparison sort
	O(n log n) does not apply.
	
---

public static void assertConcurrent(final String message, final List<? extends Runnable> runnables, final int maxTimeoutSeconds) throws InterruptedException {
  final int numThreads = runnables.size();
  final List<Throwable> exceptions = Collections.synchronizedList(new ArrayList<Throwable>());
  final ExecutorService threadPool = Executors.newFixedThreadPool(numThreads);
  try {
    final CountDownLatch allExecutorThreadsReady = new CountDownLatch(numThreads);
    final CountDownLatch afterInitBlocker = new CountDownLatch(1);
    final CountDownLatch allDone = new CountDownLatch(numThreads);
    for (final Runnable submittedTestRunnable : runnables) {
      threadPool.submit(new Runnable() {
        public void run() {
          allExecutorThreadsReady.countDown();
          try {
            afterInitBlocker.await();
            submittedTestRunnable.run();
          } catch (final Throwable e) {
            exceptions.add(e);
          } finally {
            allDone.countDown();
          }
        }
      });
    }
    // wait until all threads are ready
    assertTrue(&quot;Timeout initializing threads! Perform long lasting initializations before passing runnables to assertConcurrent&quot;, allExecutorThreadsReady.await(runnables.size() * 10, TimeUnit.MILLISECONDS));
    // start all test runners
    afterInitBlocker.countDown();
    assertTrue(message +&quot; timeout! More than&quot; + maxTimeoutSeconds + &quot;seconds&quot;, allDone.await(maxTimeoutSeconds, TimeUnit.SECONDS));
  } finally {
    threadPool.shutdownNow();
  }
  assertTrue(message + &quot;failed with exception(s)&quot; + exceptions, exceptions.isEmpty());
}

```