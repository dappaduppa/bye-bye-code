---
layout: post
title:  "Java Concurrency Notes"
date:   2017-04-13 10:28:46 +0900
categories: Java
---

// TODO:
Concurrent Set Queue

Queue with unique elements?


	
ProcessBuilder	--> to create process



## how to give interrupt to another thread ?
```java

Thread.interrupt()

// Interrupt handling code:
if (Thread.interrupted()) {
    throw new InterruptedException();
}

Thread.interrupted() --> clears the interrupt status flag.
	
Thread.isInterrupted()

// throwing InterruptedException clears the interrupt status. 
// really?
```

http://www.javaworld.com/article/2078809/java-concurrency/java-concurrency-java-101-the-next-generation-java-concurrency-without-the-pain-part-1.html

## JSR 166: Concurrency Utilities
```java
high level threading facility

    Executor framework, 
    synchronizer, 
    concurrent collections, 
    high level utility types: (semaphore, barrier, thread pools, 
                and concurrent hashmap)  --high level
    locks, 			   -- low level
    atomic variables,  -- low level
    and Fork/Join

java.util.concurrent.locks.ReentrantLock 
CAS based
( Compare and Swap )

better than 
    synchronized
```
	
http://www.javaworld.com/article/2078809/java-concurrency/java-concurrency-java-101-the-next-generation-java-concurrency-without-the-pain-part-1.html?page=2

## Get current Stack Trace:

### **in eclipse**
In eclipse set "watch expression" as  
	new Throwable().getSrackTrace()
### **via program**
```java
Programatically :
---------------
Thread.currentThread.getStackTrace() returns StackTraceElement[]

public void methodThatPrintsCaller() {
    StackTraceElement elem = Thread.currentThread.getStackTrace()[2];
    System.out.println(elem);
    // rest of code
}
sample output:
        java.lang.Thread.getStackTrace(Thread.java:1552)
        SampleRun.efgh(SampleRun.java:16)
        SampleRun.abcd(SampleRun.java:6)
        SampleRun.main(SampleRun.java:29)

---   or  ---------------------------		

final StackTraceElement[] stes = new Throwable().getStackTrace();
for (StackTraceElement ste : stes) {
    System.out.println(ste);
}
sample output:
        SampleRun.efgh(SampleRun.java:16)
        SampleRun.abcd(SampleRun.java:6)
        SampleRun.main(SampleRun.java:29)

```

//TODO  

**CopyOnWriteArrayList**

**volatile**
```java
volatile :
	use when loop variable in multithread context
	not suitable for atomic compound action.
	when one variable depends on  
	when one variable depends on another, 
	or when the new value of a variable depends on  its  old  value.  
	This  limits when  volatile  variables  are  appropriate,  
	since  they  cannot be used  to  reliably  implement 
	common tools such as counters or mutexes
```	
**synchronized method vs synchronized block**  
	synchronized block: lock could be on other than "this" object.
	
## ExecutorService    
```java
ES.shutdown()
	no new tasks accepted
	
ES.shutdownNow()
	executing tasks are stopped
	
ES.awaitTermination() 
	wait for eexcuting tasks to complete
```
