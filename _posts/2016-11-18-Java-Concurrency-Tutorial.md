---
layout: post
title:  "Java Concurrency tutorial"
date:   2016-11-18 10:45:46 +0900
categories: Java, Concurrency
---


Concurrency tutorial
--------------------

Interrupt status :


1) Thread.interrupted() -> clears interrupt status

2) t.isInterrupted() ==> (NON STATIC)
    does not clear interrupt status

Normally

    when there is
        throw new InterruptedException();

    it must be inside the block,

    Thread.interrupted() {
        throw new InterruptedException();
    }

(because once the exception is thrown, we want to clear the interrupt status.)
( i think so!)

--------------------------------------------------------------------------------


www.ibm.com/developerworks/library/j-jtp05236/

Thread.interrupt

    --> affects the following low-level blocking methods.

        * Thread.sleep
        * Thread.join
        * Object.wait

Thread.isInterrupted()
    * interrupt status read.

Thread.interrupted -->
    * interrupt status read
    * interrupt status cleared.

--------------------------------------------------------------------------------

TimeUnit

        final long noOfDays = TimeUnit.DAYS.convert(48, TimeUnit.HOURS);
        System.out.println(noOfDays);

--------------------------------------------------------------------------------

    timedJoin --> join the other thread after so many time(ex. SECONDS)...

    Thread th = new Thread(new RunnableThread());
    th.start();

    TimeUnit.SECONDS.timedJoin(th, 3L);


here DONE is printed at last after running RunnableThread.
Because the "main" thread joins the RunnableThread after 3 SECONDS.

--------------------------------------------------------------------------------
    import java.util.concurrent.TimeUnit;

    public class TimeUnitSample {

        public static void main(String[] args) throws InterruptedException {

            final long noOfDays = TimeUnit.DAYS.convert(48, TimeUnit.HOURS);
            System.out.println(noOfDays);
            System.out.println(System.currentTimeMillis());

            //TimeUnit.SECONDS.sleep(2L);

            Thread th = new Thread(new RunnableThread());
            th.start();

            TimeUnit.SECONDS.timedJoin(th, 3L);

            System.out.println(">>>>>>>>>DONE"); // --------------------------->
                                                 // PRINTED at last

        }
    }

    class RunnableThread implements Runnable {

        @Override
        public void run() {
            int cnt = 0;
            for (; cnt < 5; cnt++) {
                System.out.println(">>> run :" + cnt);
            }

            try {
                TimeUnit.SECONDS.sleep(2L);
            } catch (final InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

--------------------------------------------------------------------------------

CopyOnWriteArrayList
--------------------

for mutative operations like set, add, ...
the backing array is copied and recreated during when the iterator is created.

please refer sample,
MyCopyOnWriteArrayList.java

```java
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.CopyOnWriteArrayList;
import java.util.concurrent.TimeUnit;

class SampleRun implements Runnable {

    private List<String> list;

    SampleRun(List<String> list) {
        this.list = list;
    }

    @Override
    public void run() {
        try {
            TimeUnit.MILLISECONDS.sleep(250);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        list.add("isitnot");
    }
}

public class MyCopyOnWriteArrayList {

    public static void main(String[] args) throws InterruptedException {

        final List<String> list = new ArrayList<>();
        list.add("this");
        list.add("is");
        list.add("fun");

        final Thread th = new Thread(new SampleRun(list));
        th.start();

        final List<String> listCopy = new ArrayList<>(list);
        printList(listCopy);           // comment
        printList(list);                                     // uncomment and test




        try {
            TimeUnit.SECONDS.timedJoin(th, 10L);
        } catch (final InterruptedException e) {
            throw e;
        }

        System.out.println(">>>>>>>after run over");

        printList(list);

        System.out.println(">>>>>>>list copied./....");

        printList(listCopy);

    }

    private static void printList(final List<String> list) {
        for (final String string : list) {
            try {
                TimeUnit.MILLISECONDS.sleep(250);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(string);
        }

    }
}

```
--------------------------------------------------------------------------------
when writing concurrency test, plan to sleep a while goiing through the list,
so that in testing chances of finding the "ConcurrentModificationException" is
more.


    for (final String string : list) {
            try {
                TimeUnit.MILLISECONDS.sleep(250);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(string);
        }

--------------------------------------------------------------------------------

See concurrent classes
    * CyclicBarrier
    * ReentrantLock
    * CountDownLatch

"CyclicBarrier is NOT working?" --> StackOverflow
## CyclicBarrierTest.java
```java
import java.util.Arrays;
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;
import java.util.concurrent.TimeUnit;

class Solver {
    final int N;
    final float[][] data;
    //boolean done = false;
    final CyclicBarrier barrier;

    class Worker implements Runnable {
        boolean done = false;
        int myRow;

        Worker(int row) {
            myRow = row;
        }

        public void run() {
            while (!done) {
                processRow(myRow);

                try {
                    System.out.println(">>>>>>>>>>>first");
                    barrier.await();
                    System.out.println(">>>>>>>>>>>second");
                } catch (InterruptedException ex) {
                    return;
                } catch (BrokenBarrierException ex) {
                    return;
                }
            }
            System.out.println("Run finish for " + Thread.currentThread().getName());
        }

        private void processRow(int row) {

            float[] rowData = data[row];

            for (int i = 0; i < rowData.length; i++) {
                rowData[i] = 1;
            }
//
//            try {
//                Thread.sleep(2000);
//            } catch (InterruptedException e) {
//                e.printStackTrace();
//            }
            done = true;
        }
    }

    public Solver(float[][] matrix) {
        data = matrix;
        N = matrix.length;
        barrier = new CyclicBarrier(N, new Runnable() {
            public void run() {

//              try {
//                  TimeUnit.SECONDS.sleep(2l);
//              } catch (InterruptedException e) {
//                  // TODO Auto-generated catch block
//                  e.printStackTrace();
//              }
//
                for (int i = 0; i < data.length; i++) {
                    System.out.println("Data " + Arrays.toString(data[i]));
                }

                System.out.println("Completed:");
            }
        });
        for (int i = 0; i < N; ++i)
            new Thread(new Worker(i), "Thread "+ i).start();
    }
}

public class CyclicBarrierTest {
    public static void main(String[] args) {

        float[][] matrix = new float[5][5];

        Solver solver = new Solver(matrix);
    }
}
```

* Even in this example, it is the "Thread.Sleep" used for avoiding the bug.
 but in high load the bug may happen.

* Thread.sleep can be used to find/simluate bugs in concurrent systems.

--------------------------------------------------------------------------------

Guarded blocks:
----------------
    * exit loop when the condition is met
    * inside loop, call wait()

    while(!joy) {} --> dont do this, wasting processor time

    correct way of guarded block:
    -------------
        synchronized {
            while(!joy) {wait()}
        }

wait , notifyAll, synchronized


on message has to be consumed before the next message is produced....

## Drop.java
## Producer.java
## Consumer.java
## ProducerConsumerSample.java

## Drop.java
public class Drop {
    // Message sent from producer
    // to consumer.
    private String message;
    // True if consumer should wait
    // for producer to send message,
    // false if producer should wait for
    // consumer to retrieve message.
    private boolean empty = true;

    public synchronized String take() {
        // Wait until message is
        // available.
        while (empty) {
            try {
                wait();
            } catch (InterruptedException e) {
            }
        }
        // Toggle status.
        empty = true;
        // Notify producer that
        // status has changed.
        notifyAll();
        return message;
    }

    public synchronized void put(String message) {
        // Wait until message has
        // been retrieved.
        while (!empty) {
            try {
                wait();
            } catch (InterruptedException e) {
            }
        }
        // Toggle status.
        empty = false;
        // Store message.
        this.message = message;
        // Notify consumer that status
        // has changed.
        notifyAll();
    }
}
```
## Producer.java
```java
import java.util.Random;

public class Producer implements Runnable {
    private Drop drop;

    public Producer(Drop drop) {
        this.drop = drop;
    }

    public void run() {
        String importantInfo[] = { "Mares eat oats", "Does eat oats",
                "Little lambs eat ivy", "A kid will eat ivy too" };
        Random random = new Random();

        for (int i = 0; i < importantInfo.length; i++) {
            drop.put(importantInfo[i]);
//          try {
//              Thread.sleep(random.nextInt(5000));
//          } catch (InterruptedException e) {
//          }
        }
        drop.put("DONE");
    }
    public static void main(String[] args) {
        Random random = new Random();
        for(;;)
            System.out.println(random.nextInt(5000));
    }
}
```
## Consumer.java
```java
import java.util.Random;

public class Consumer implements Runnable {
    private Drop drop;

    public Consumer(Drop drop) {
        this.drop = drop;
    }

    public void run() {
        Random random = new Random();
        for (String message = drop.take(); !message.equals("DONE"); message = drop
                .take()) {
            System.out.format("MESSAGE RECEIVED: %s%n", message);
//          try {
//              Thread.sleep(random.nextInt(5000));
//          } catch (InterruptedException e) {
//          }
        }
    }
}
```
## ProducerConsumerSample.java
```java
public class ProducerConsumerExample {
    public static void main(String[] args) {
        Drop drop = new Drop();
        (new Thread(new Producer(drop))).start();
        (new Thread(new Consumer(drop))).start();
    }
}
```

    public synchronized void put(String message) {  --> synchornoized to acquire intrinsic lock


        // Wait until message has
        // been retrieved.
        while (!empty) {
            try {
                wait();
            } catch (InterruptedException e) {
            }
        }

-----

CyclicBarrier constructor arguments
    1) no of parties(threads)
    2) runnable action.. => run once the parties thread started.
        remember runnable action will start NOT at the end of parties run.
            it will run once the parties started.
refer sample
C:\dhans\ws\CollectionWS\SampleProject\ddd\CBarSample.java

--------------------------------------------------------------------------------

semaphore in java

semaphore instance.acquire() --> lock start
... do something
...
semaphore instance.release() --> lock release


1.3.2. Liveness hazards:
------------------------

    liveness failures:
        * Single threaded progam : infinite loop
        * Multithreaded program  :
                                1) deadlock
                                2) Starvation
                                3) livelock


---
unlucky timing on accessing data in threading called as
--> race condition


* data race
* race condition

public class Widget {
    public synchronized void doSomething() {
    ...
    }
}

public class LoggingWidget extends Widget {
    public synchronized void doSomething() {
        System.out.println(toString() + ": calling doSomething");
        super.doSomething();
    }
}


* doubt: but lock on which object,

Suppose, if we are creating instance of "LoggingWidget", calling
doSomething() method, acquires "Widget" lock or "LoggingWidget"???

--------------------------------------------------------------------------------


locking guarantee
    --> (1) atomicity
        (2) visibility

volatile guarantees ONLY
        1. visibility



--------------------------------------------------------------------------------

Inner classes expose hidden reference to Outerclass
    Please refer : "20161129_jcip.txt.png"
```java

public class ThisEscapeSample {

    private class MyInnerClass {

    }


    public MyInnerClass getMe() {
        return new MyInnerClass();
    }

    public static void main(String[] args) {

        MyInnerClass myInnerClass = new ThisEscapeSample().getMe();
        System.out.println(myInnerClass);
        // debug here ... myInnerClass --> has reference to ThisEscapeSample
    }
}

```
ThreadLocal ???
    I dont get it???

--------------------------------------------------------------------------------
4.2. Instance Confinement:
--------------------------

Set is NOT threadsafe,
But PersonSet is threadsafe.


@ThreadSafe
public class PersonSet {
    @GuardedBy("this")
    private final Set<Person> mySet = new HashSet<Person>();

    public synchronized void addPerson(Person p) {
        mySet.add(p);
    }

    public synchronized boolean containsPerson(Person p) {
        return mySet.contains(p);
    }
}

--------------------------------------------------------------------------------

prefer private object lock
    instead of object's intrinsic lock.

    public class PrivateLock {
        private final Object myLock = new Object();
        @GuardedBy("myLock")
        Widget widget;

        void someMethod() {
            synchronized (myLock) {
                // Access or modify the state of widget
            }
        }
    }

--------------------------------------------------------------------------------


Concurrency tutorial:
---------------------

//TODO
    Study about "ProcessBuilder" api for creating new process.


* create Thread Object -> direct
* use "Executor"       -> Abstract thread mgmt (from Application)


// TODO
    explore the static methods in the thread class.


Thread contention:
------------------

    when 2 or more threads access the same resource,
    java runtime executes 1 or more threads slowly,
    even suspend their execution.


    Thread contention forms/types:
    -----------------------------
    Liveliness:
    -----------
        1) Stravation
        2) Livelock


Synchronizing constructors does not make sense.:
------------------------------------------------
 it is syntax error.

    Because thread creating constructor has the sole access to the object.
    Got it??!!!


Give me an example of an object accessed before it is created completely?
-------------------------------------------------------------------------

    If you maintain the list of objects in the list,

    Inside the constructor,

    for example,

    public Sample() {
        instances.add(this);
        ...
        ...
    }

    then, the current object is visible via "instances" list
    before created completely.

Between multiple threads, "final" fields can be safely accessed without
"synchronization".

-------------------------

reentrant synchronization:
--------------------------

    acquire the lock on the same thread.

Interleave access to fields that are not needed for synchronization together.
-----------------------------------------------------------------------------

public class StmtSendReceive {
    private int recd;
    private int sent;

    private Object recdObject = new Object();
    private Object sentObject = new Object();


    public void recdPlusPlus {
        synchronized(recdObject) {
            recd++;
        }
    }

    public void sentPlusPlus {
        synchronized(sentObject) {
            sent++;
        }
    }


}


Atomic access:
--------------
    * all primitives read, write ( except long, and double)
    * all primitives read, write declared "volatile"(including long,and double)


* ability to execute in a timely manner --> liveness.
    1) most common liveness problem --> deadlock
    2) other liveness problem       --> starvation and livelock


starvation :
------------
    * casued by greedy threads so that other threads are starving
        for shared resource.

livelock:
----------
    * not a deadlock,
    but both are responding to each other to resume.

    important thing to note: not doing action completely.
                             it is the position (
                             just after the dealock as if happened)
                             and before completing action,
                             another thread started doing action and stops
                             execution of prev. thread.

    bowing 2 fellows continuously.( but not bowing completely.,
                                    little bowing bowing...)

notify , wait
--------------
    * notify and wait must be called within the synchronized block.
    * otherwise IllegalMonitorStateException occurs.

    notify
    (vs)
    notifyAll
Prefer notifyAll() ...why ?
    because notify() you can not guarantee which thread will be woken up.


PAGE 188...

----------------

java concurrency tutorial;

Synchronized class example:
---------------------------

    Could not understand the function name getRGB
    basically does the following return

    return (red << 16 | green << 8 | blue);

        int r = 255;
        int g = 0;
        int b = 0;

        System.out.println(r << 16 | g << 8 | b);
        rgb(0,0,0) --> black
        rgb(255, 255, 255) --> white


Strategy for defining immutable objects:
----------------------------------------

1) define member fields as private, final
2) no setter methods.
3) stop all the ways to manipulate member vars.
4) declare class final.
5) make class Singleton.


High level concurrency objects:
-------------------------------

1) Lock objects
    lock vs intrinsic lock
        timeout possible,
        tryLock --> acquires the lock only if it is free.
        *with intrinsic lock (synchronized), this is NOT possible.

2) Executors
3) Concurrent collections
4) Atomic variables
5) ThreadLocalRandom

-----------
High level concurrency objects:
-------------------------------

1) Lock objects
    lock vs intrinsic lock
        timeout possible,
        tryLock --> acquires the lock only if it is free.
        *with intrinsic lock (synchronized), this is NOT possible.
        * biggest advantage :ability to backout of an attempt to acquire a lock.


2) Executors
    * Isolate and thread mgmt from the rest of the application.
    1) Executor Interfaces --> three executor object types.
        a) Executor Interface
            (new Thread(r)).start(); // r --> Runnable Object

                is equal to

            e.execute(r); // e = executor implementation
                * executor implementaion may use existing "worker" threads
                  to execute "r"
                  (OR)
                * to place "r" in the queue to wait for worked thread to become
                  available.

        b) ExecutorService extends Executor
            Manages lifecyle of
                * Individual task
                * executor itself.

            Future f = es.submit(r);
            Future f = es.submit(c); // c = Callable Object

            * Future object is used to
                    * retrieve the Callable return value.
                    * also manages the status of "Callable" and "Future" objects.



        c) ScheduledExecutorService extends ExecutorService
            supports taks of...
                * "future" execution
                * "periodic" execution

            * scheduleAtFixedRate
            * scheduleWithFixedDelay


    2) Thread Pools        ==> most common executor implementation
    3) Fork/Join (JDK7)    --> taking advantage of multiple processors.


3) Concurrent collections
4) Atomic variables
5) ThreadLocalRandom


-----------



