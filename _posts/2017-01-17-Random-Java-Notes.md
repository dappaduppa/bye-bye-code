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

--------java general

## 	list class loading information
```java
java -verbose:class com.Test >> abcd.log 2>&1
	
```
## Java Secure Channel
```java
JSch ( Java Secure Channel )
ssh2 for java
can be used for various execution of commands using Runtime.getRuntime().exec(<command/>)
```

## floating point arithmatic
```java
study numerical computational guide from Sun
"What every computer scientist should know about Floating-Point arithmetic"


sequence points ( ensure the seqence of execution )

i=i++ ( not sure how it will behave)
```

## tools
```java
-----------
tools to find out what the system is doing

  jprof
  jcover

there should be a tradeoff between the two
--------------------
lock overhead
lock contention
```

## java home
```java
expecting "fun", but no "fun". only bun...hehehehe why? 
$ JAVA_HOME="fun" echo $JAVA_HOME
/usr/java7_64
```

## regex
```java
Exceptions in a character set 
[0-9]] => any no. followed by "]"
[]0-9] => "]" or any no.
```

## enum notes
```java
public enum Abcd1 {
    THIS("this"), IS("is"), FUN("fun");

    private final String abcdStr;

    Abcd1(final String abcdStr) {
        this.abcdStr = abcdStr;
    }

    public String getValue() {
        return this.abcdStr;
    }
}

// Usage / mistakes:
(Abcd1.FUN == Abcd1.valueOf(Abcd1.FUN.toString()))  => true

    or

(Abcd1.FUN == Abcd1.valueOf("FUN")  => ok
(Abcd1.FUN == Abcd1.valueOf("fun")  => NOT OK.
        IllegalArgumentException....
        No enum constant Adcd1.fun

```
##Stackoverflow qn

```java
How l can call values ??() from instance of generic type <E extends Enum <E >>?

public static <E extends Enum<E>> void example(E e){ e. //what should I put here to get result of values()? }


I Am Trying To I Call The Method values() That Returns The Values Of An enum Using This Method With Generic Arguments.

 How can I do that?
```
 ## Answer

```java
Enum Does Not Have values() Method. It Is Added By Compiler Later To Each Of Your Enum Subclasses (When We Are Creating Our Own enum YourEnum{...} Type). So We Can Not Invoke values() From Enum Type.

 Possible solution is
 E[] values = e.getDeclaringClass().getEnumConstants();

 Be Safe We To Should Use getDeclaringClass() Instead Of getClass() , Because Enum Values Can Be Implemented As Anonymous Classes Like In Case Of TimeUnit
 public enum TimeUnit { /** * Time unit representing one thousandth of a microsecond */ NANOSECONDS { public long toNanos(long d) { return d; } public long toMicros(long d) { return d/(C1/C0); } public long toMillis(long d) { return d/(C2/C0); } public long toSeconds(long d) { return d/(C3/C0); } public long toMinutes(long d) { return d/(C4/C0); } public long toHours(long d) { return d/(C5/C0); } public long toDays(long d) { return d/(C6/C0); } public long convert(long d, TimeUnit u) { return u.toNanos(d); } int excessNanos(long d, long m) { return (int)(d - (m*C2)); } },

 In this case
?  TimeUnit.NANOSECONDS.getClass() Returns java.util.concurrent.TimeUnit$3 (But This Anonymous Class Does Not Contain Any Enum Values)
?  TimeUnit.NANOSECONDS.getDeclaringClass() Returns java.util.concurrent.TimeUnit (With Contains All Of Enum Values).

 It will also save us some trouble with casting since
?  getDeclaringClass Returns Class<E>
?  getClass() Returns Class<? extends E>

 So result type of
?  getDeclaringClass().getEnumConstants() Is E[]
?  While getClass().getEnumConstants() Is ? extends Enum[] .

 This allow us to create loop like without need to involve casting:
 for (E value : e.getDeclaringClass().getEnumConstants()){ //handle value }


 An Extra As, You Can Also Get All Values From Class<E> (Where <E extends Enum<E>> ) By Using EnumSet.allOf(enumClass) Like In Case Of Code From This Question:
 EnumSet<E> allOf = EnumSet.allOf(e.getDeclaringClass());




 Side Note Interesting: getEnumConstants() Is Actually Implemented By Calling values() In The Enum Class Via Reflection . :-) (The Reflection Happens Only Once Per Class: The Values Are Cached, And getEnumConstants() Returns A Defensive Copy Of Those Values . On Each Call) - Chris Jester-Young Jun 11 At 16:46


```

## eclipse Side Note:
```txt
  In eclipse, writing a file like
        final File file = new File("sample.out");

 writes the file under the "PRoject" folder.
```

## aix and windows CRLF
```txt
Today I downloaded from rmine.


Happily uploaded to AIX in ascii format and compare the size in
windows and AIX.

Voila ! It is same size. ( I expected the size should be small,
since CRLF will be replaced with LF,
but this does not happen, Since earlier the source released with aix format.

i.e. from AIX itself pack is built and delivered.
)

But download from AIX to windows, size increased !
since "ftp" program function add "CR" this time.

I suggest my mind to stick with one format., aix format.
so that no more confusion on the size part
when mentioning in FSThread per task approach:
--------------------------
    seems to work in dev. and testing .
    but rememember it will not work in real time / production.
``` 

## ssh
```txt
ssh login script

connect 'ip:port_no /ssh /2 auth... pwd...'

instead of the simple connect
connect --> ( by default ) connects to the "telnet"

connect /ssh --> connects to ssh
    // actually slice header is passed.
```

## ORACLE size
```sql
SET PAGESIZE 5000
SET LINESIZE 200
COLUMN segment_name FORMAT A40

SELECT segment_name, SEGMENT_TYPE, bytes/1024/1024/1024 as "GB"
from user_segments
order by bytes/1024/1024/1024 desc;
```

## StackOverFlowError on toString():

```txt
Item 2: Consider a builder when faced with many constructor parameters.
    Note: out of context, toString() method, should not call any var, method that
    indirectly call toString() method !

    StackOverFlowError occurs.!!!
```

## arraylist vs linkedlist in java.
// TODO

## int, currency
```java

    java8
        int, long

        compareUnsigned
        divideUnsigned

java.math.BigDecimal for currency
```

## index
```sql

SQL> SET PAGESIZE 5000
SET LINESIZE 200
COLUMN TABLE_NAME FORMAT A40
COLUMN INDEX_NAME FORMAT A40
COLUMN STATTYPE_LOCKED FORMAT A10SQL> SQL> SQL> SQL>
SQL>
SQL> SELECT INDEX_NAME, TABLE_NAME, STATUS FROM USER_CONSTRAINTS WHERE TABLE_NAME = 'FSGL_CHEQ012';

INDEX_NAME                               TABLE_NAME                               STATUS
---------------------------------------- ---------------------------------------- ------------------------
                                         FSGL_CHEQ012                             ENABLED
PK_FSGL_CHEQ012                          FSGL_CHEQ012                             ENABLED

SQL>
SELECT INDEX_NAME, TABLE_NAME, STATUS FROM USER_CONSTRAINTS WHERE TABLE_NAME = 'FSGL_CHEQ012';

ALTER INDEX PK_FSGL_CHEQ012 REBUILD ONLINE;


SELECT COUNT(*) FROM FSGL_CHEQ012;


Enter user-name: T24
Enter password:

Connected to:
Oracle Database 11g Enterprise Edition Release 11.2.0.4.0 - 64bit Production
With the Partitioning, Real Application Clusters and Automatic Storage Management options

SQL>
SQL>
SQL> set autotrace on
SQL>
SQL> SELECT COUNT(*) FROM FSGL_CHEQ012;

  COUNT(*)
----------
   1997143


Execution Plan
----------------------------------------------------------
Plan hash value: 468176749

----------------------------------------------------------------------------
| Id  | Operation        | Name            | Rows  | Cost (%CPU)| Time     |
----------------------------------------------------------------------------
|   0 | SELECT STATEMENT |                 |     1 |     1   (0)| 00:00:01 |
|   1 |  SORT AGGREGATE  |                 |     1 |            |          |
|   2 |   INDEX FULL SCAN| PK_FSGL_CHEQ012 |  3414K|     1   (0)| 00:00:01 |
----------------------------------------------------------------------------


Statistics
----------------------------------------------------------
          1  recursive calls
          0  db block gets
       9723  consistent gets
          0  physical reads
          0  redo size
        529  bytes sent via SQL*Net to client
        524  bytes received via SQL*Net from client
          2  SQL*Net roundtrips to/from client
          0  sorts (memory)
          0  sorts (disk)
          1  rows processed

SQL>


----------

SQL>
SQL> SELECT INDEX_NAME, TABLE_NAME, STATUS FROM USER_CONSTRAINTS WHERE TABLE_NAME = 'FSGL_CHEQ012';

INDEX_NAME                               TABLE_NAME                               STATUS
---------------------------------------- ---------------------------------------- ------------------------
                                         FSGL_CHEQ012                             ENABLED
PK_FSGL_CHEQ012                          FSGL_CHEQ012                             ENABLED

SQL>
SQL> ALTER INDEX PK_FSGL_CHEQ012 REBUILD ONLINE;

Index altered.

SQL> SELECT INDEX_NAME, TABLE_NAME, STATUS FROM USER_CONSTRAINTS WHERE TABLE_NAME = 'FSGL_CHEQ012';

INDEX_NAME                               TABLE_NAME                               STATUS
---------------------------------------- ---------------------------------------- ------------------------
                                         FSGL_CHEQ012                             ENABLED
PK_FSGL_CHEQ012                          FSGL_CHEQ012                             ENABLED


CT VOC FSGL.CHEQUE.REGISTER.SUPPLEMENT
    FSGL.CHEQUE.REGISTER.SUPPLEMENT
001 TABLE
002 XMLORACLEInit
003 D_F_CHEQUE_000 NOXMLSCHEMA
004 FSGL_CHEQ012

SQL>  SELECT CONSTRAINT_NAME, CONSTRAINT_TYPE, INDEX_NAME FROM USER_CONSTRAINTS WHERE TABLE_NAME ='FSGL_CHEQ012';

CONSTRAINT_NAME                                                                            CON INDEX_NAME
------------------------------------------------------------------------------------------ --- ----------------------------------------
SYS_C00664398                                                                              C
PK_FSGL_CHEQ012                                                                            P   PK_FSGL_CHEQ012


ALTER TABLE FSGL_CHEQ012 DROP CONSTRAINT PK_FSGL_CHEQ012 DROP INDEX;
ALTER TABLE FSGL_CHEQ012 ADD CONSTRAINT PK_FSGL_CHEQ012 PRIMARY KEY(RECID);



CREATE TABLE COPY_FSGL_CHEQ012 AS ( SELECT * FROM FSGL_CHEQ012);
alter table FSGL_CHEQ012_copy RENAME TO FSGL_CHEQ012;


SELECT CONSTRAINT_NAME, CONSTRAINT_TYPE, INDEX_NAME FROM USER_CONSTRAINTS WHERE TABLE_NAME ='COPY_FSGL_CHEQ012';
--ALTER TABLE COPY_FSGL_CHEQ012 DROP CONSTRAINT PK_FSGL_CHEQ012 DROP INDEX;
ALTER TABLE COPY_FSGL_CHEQ012 ADD CONSTRAINT PK_COPY_FSGL_CHEQ012 PRIMARY KEY(RECID);


ALTER INDEX PK_COPY_FSGL_CHEQ012 REBUILD ONLINE;

```

## dont just copy paste , param values
```java
// copying 2000 to TimeUnit will not work as expected,
// since Thread.sleep(in millis)
// TimeUnit.Seconds...(in secs)

// Dont directly copy the no. and expect the same behaviour.
Thread.sleep(2000); --> 2 secs
TimeUnit.SECONDS.sleep(2000); --> 2000 secs
```

## tablespace
```sql


sqlplus T24SYSADM/T24SYSADMPAS0@ITWT24_SERVICE
SQL> set line 1000;
SQL> set pagesize 500;
SQL> select
        d.tablespace_name,
        現サイズ "現サイズ[MB]",
        round(現サイズ-空き容量) "使用量[MB]",
        round((1 - (空き容量/現サイズ))*100) "使用率(%)",
        空き容量 "空き容量[MB]"
from
(SELECT tablespace_name, round(SUM(bytes)/(1024*1024)) "現サイズ"
FROM dba_data_files GROUP BY tablespace_name) d,
(SELECT tablespace_name, round(SUM(bytes)/(1024*1024)) "空き容量"
FROM dba_free_space GROUP BY tablespace_name) f
where d.tablespace_name=f.tablespace_name ORDER BY "使用率(%)" DESC;

------------------------------------------------------------------------------------------ ------------ ---------- ---------- ------------
TABLESPACE_NAME                                                                            現サイズ[MB] 使用量[MB]  使用率(%) 空き容量[MB]
------------------------------------------------------------------------------------------ ------------ ---------- ---------- ------------
T24DATA                                                                                          917497     746718         81       170779
T24INDEX                                                                                          98301      75170         76        23131
SYSTEM                                                                                             2048       1207         59          841
SYSAUX                                                                                             3072       1421         46         1651
ORAWORKDATA                                                                                        2111        855         41         1256
DMTINDEX                                                                                         153090      57169         37        95921
IFBINDEX                                                                                          12800       3670         29         9130
DMTDATA                                                                                          171012      46222         27       124790
UNDO1                                                                                             65534      12686         19        52848
IFBDATA                                                                                           18944       3017         16        15927
T24AQDATA                                                                                         22528       3492         16        19036
IFOINDEX                                                                                           2560         93          4         2467
WAS_SESSION_POOL                                                                                    100          1          1           99
IFODATA                                                                                           12288         65          1        12223



------------------------------------------------------------------------------------------ ------------ ---------- ---------- ------------
50
```

## excel bahttext
```excel
=BAHTTEXT(1)
```
## sin cos tan

```txt
practical use of Sin and Cos????


                    /|
                   / |
                  /  |
       hyptenuse /   | opposite
                /    |
               /)____|
              p1  adjacent

tan0 = opp/adj
sin0 = opp/hyp
cos0 = add/hyp


calculate height of building without actually measuring height.
* angle a light from position p1 to the top of building.
* measure the distance from the base of the building to position p1
* now,
    tan(deg) = opp / adj

        -> deg,
        -> adj
        are known parameters.
    Now calculate the height of the buliding

    opp     = tan(angle in deg) * adj
    (height)

```




/******************************************************************************/
```
## java threading
```java
Thread.currentThread().getName()

/******************************************************************************/
package zig;


public class Sample {
    public static void main(String[] args) {
        System.out.println("this is zig Sample");

        final SampleThread st = new SampleThread();
        final Thread t = new Thread(st);
        t.start();

        t.run();

        final Thread t1 = new Thread(st);
        t1.start();
    }
}



class SampleThread implements Runnable {

    public void run() {
        System.out.println(">>>>>>>>" + Thread.currentThread());
    }

}



$ java zig.Sample
this is zig Sample
>>>>>>>>Thread[main,5,main]
>>>>>>>>Thread[Thread-0,5,main] --> thread name, priority, threadgroup
>>>>>>>>Thread[Thread-1,5,main]


/******************************************************************************/


Thread.toString() --> thread name, priority, threadgroup

//TODO example of Other than thread group --> main

//TODO
volatile
jconsole
kill -3
read thread analysis already done.
prepare real time issues, with vmstat, thread dump


/******************************************************************************/

synchronized method1 (bow)
    |
    | calls
    |
    +---->  synchronized method2 (bowback)

sure deadlock ...

if two threads accessing

    one thread at method2 ...exit stage ....
        at that time if the next thread locks method1

        deadlock....

//TODO
how to avoid...

/******************************************************************************/

ProcessBuilder object to create additional process...

//TODO multi process application

//TODO Thread.sleep usage ???

2
3(QUIT) for dump
9
15

kill -INT
kill -QUIT
kill -KILL
kill -TERM
```

good site on threads
yegor256.com





-------------------------------------------------------------------------------

```

## jenkins
```java
supported build tools:
-----------------------

    * Maven
    * Gradle

Spring & Grails uses
    Gradle build tool

android also used gradle build tool


favor @Rule ExpectedException
over
@Test ( expected = Exception.class)
since Exception may occur at one of the lines in the test,
but test case may pass.

sample: precise location of exception occuring point can be tested with
@Rule.

but with @Test(expected = SomeException.class),
code can throw exception at any point, so the intended test is not complete by
itself.
--------------------------------------------------------------------------------
    @Rule
    public ExpectedException thrown = ExpectedException.none();

    @Test//(expected = IndexOutOfBoundsException.class)
    public void test() {
        final List<String> list = new ArrayList<>();
        final int[] intArr = { 1, 2, 3 };
        final int x = intArr[2];
        assertEquals(3, x);

        thrown.expect(IndexOutOfBoundsException.class);
        thrown.expectMessage("Index: 1, Size: 0");
        list.get(1);
    }
--------------------------------------------------------------------------------


template for test

@Test
public void shouldDoThis() {
    // given

    // when

    // then

}



missing super.basemethod() means
in the derived class we are doing completely different.
then composition has to be used.

if the derived class instance is assigned with the base class,
and calling the base class method should work as a base class
( obviously with NO additional feautures provied in the derived class),
otherwise we can say that derived class is NOT intended for inheritance.
better use composition in this case.


```

## 2016117_monkeyisland.pl.txt
```java
cascading when hibernating:
    transitive persistence is NOT default


    session.save(book)

        needs to be done as

    session.save(book.getPublication());
    session.save(book.getAuthor());
    session.save(book);

assuming each one, "Publication", "Author", and "Book"
representing separate tables.
"book" to each table relationship is "one to　many".


---
EasyMock to Mockito
---


State based test --> classicists


tests on behviour --> mock objects.

I dont see any difference....martin fowler fool write some scrappy stuff.
mocks are not stubs...
may be i am not understanding the difference.

--------------------------------------------------------------------------------

running Junit with repeatable set of parameters.

class level declaration @RunWith(value ="Parameterized.class")

define
Constructor and member variables.
( variables representing  input and expected values. )

define static method

@Parameters(name= "test case param index ={index} , input = {0}, output = {1}")
public static Collection<Integer> data() { // method name can be anything
    return Arrays.asList(
                        new Integer[][]{
                            {2, 8},
                            {7, 128}
                        }
    );
}

--------------------------------------------------------------------------------


how jUnit works???

even the previous example(Parameterized.class) ??

--------------------------------------------------------------------------------

javadoc
/** **/

@author author-name
@version version number of class
@see String/URL/classname#methodname


@see #methodname
    method defined in the same file.

@param param_name description
@return description of return value.
@exception exception_name description


--------------------------------------------------------------------------------

ExecutorService es1 = Executors.newFixedThreadPool(10);

ExecutorService es2 = Executors.newSingleThreadExecutor();

ExecutorService es3 = Executors.newScheduledThreadPool(20);
```


## mass

```
W = mg

g = 9.8026527433371290385466344829608

  = G * mass / radius * radius

    // universal gravitational constant  (m3 kg-1 s-2)
    public static final double G = 6.67300E-11;

             mass       radius
    EARTH   (5.976e+24, 6.37814e6),


175 kg = ? mass

W = mg
=>
 m = W / g
   = 175 / 9.8026527433371290385466344829608

   = 17.852310449224842825019043153984


   = kg * / m

   -----------------------------------------
Planet.java

Your weight on MERCURY is 27.198548
Your weight on VENUS is 65.159935
Your weight on EARTH is 72.000000
Your weight on MARS is 27.269077
Your weight on JUPITER is 182.200142
Your weight on SATURN is 76.753119
Your weight on URANUS is 65.169158
Your weight on NEPTUNE is 81.959621


Dont you think:
---------------

My weight in Earth is 72 Kg.
almost the same nearest weight is on "Saturn" ==> 76.75 Kg.

Forgetting the factors of water, oxygen, and other resources,
considering the Mind/weight balance, will "Saturn" be the the place,
human can adopt easily.

I am not sure about natural resources like water, oxygen in "Saturn"


```

