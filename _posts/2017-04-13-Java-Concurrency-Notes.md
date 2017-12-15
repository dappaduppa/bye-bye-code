---
layout: post
title:  "Java Concurrency Notes"
date:   2017-04-13 10:28:46 +0900
categories: Java
---

// TODO:
Concurrent Set Queue

Queue with unique elements?


	
## ProcessBuilder	--> to create process


### ProcessBuilder:

```java
    java.lang.ProcessBuilder
          Map<String, String>  -> environment()
          directory(dirname) --> set the pb working dir
                                basically run the process in this dir
```

```java
        ProcessBuilder pb = new ProcessBuilder("abcd1.exe", "myArg1", "myArg2");
        Map<String, String> env = pb.environment();
        env.put("VAR1", "myValue");
        env.remove("OTHERVAR");
        env.put("VAR2", env.get("VAR1") + "suffix");
        pb.directory(new File("ds"));
        File log = new File("log");
        pb.redirectErrorStream(true);
        pb.redirectOutput(Redirect.appendTo(log));
        Process p = pb.start();
        assert pb.redirectInput() == Redirect.PIPE;
        assert pb.redirectOutput().file() == log;
        assert p.getInputStream().read() == -1;
```


## vm arguments
    -Xms1g -Xmx2g
    (**no** semicolon)
    s = initial
    x = max
    g => not work in 32 bit
    -d64 = 64 bit runtime

## rt>exec() vs pb>start()

```java
    Runtime.getRuntime().exec("C:\DoStuff.exe -arg1 -arg2");

    // not ok
    ProcessBuilder b = new ProcessBuilder("C:\DoStuff.exe -arg1 -arg2");
    //Instead, you should use it as.
    //Remember always pass each parma as separate string(arg)
    ProcessBuilder b = new ProcessBuilder("C:\DoStuff.exe", "-arg1", "-arg2");

    or alternatively
    List<String> params = java.util.Arrays.asList("C:\DoStuff.exe", "-arg1", "-arg2");
    ProcessBuilder b = new ProcessBuilder(params);

```

## running two process
```java
    // will not work.
    pb = new ProcessBuilder("javac","Mocha.java","&&","java","Mocha");

    // split it two
      pb2 = new ProcessBuilder("javac","Mocha.java");
      Process p = pb.start();
      int err = p.waitFor();
      p.destroy();
      if (err==0) {
        pb2 = new ProcessBuilder("java", "Mocha");
      }
```

## read pb output
```java
    InputStream is = process.getInputStream();
    InputStreamReader isr = new InputStreamReader(is);
    BufferedReader br = new BufferedReader(isr);
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }

    // better use Apache IOUtils from Apache IO commons.
    IOUtils.toString(inputStream, YOUR_CONSOLE_CHARSET);
```

##  java.util.zip ? use case?

watch out for argument "-" to process builder with out spaces " - "
because shell normally omit the space.
so space should not be there in " - ".( use "-")

processbuilder.start() --> Process
    Process -> inputstream => output of pb exe
            -> outputsream =>

## processbuilder.process input stream vs output stream

```


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


## interrupt
```java
public class MyRunnable implements Runnable {

    @Override
    public void run() {
        while(!Thread.currentThread.isInterrupted()) {
            // heavy operation
            try {
                Thread.sleep(2000);
            } catch (final InterruptedException e) {
                Thread.currentThread.interrupt(); // --> set interrupt to exit from loop
                // or say continue
            }
        }
    }

}
```

## SingleThreadWebServer ( actually modified to support multiple threads )

```java

import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

public class SingleThreadWebServer {
    public static void main(String[] args) throws IOException {
        final ServerSocket ss = new ServerSocket(80);
        while (true) {
            Socket s = null;
            try {
                s = ss.accept();
                s.close();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
            new Thread(new Runnable() {
                @Override
                public void run() {

                    System.out.println(">>>>>>>>");
                }
            }

            ).start();
        }
    }
}

```


## remember this:
```txt
ServerSocket ( create ServerSocket with port)
Socket ( ServerSocket.accept() results socket)

if we are NOT closing the socket the response wont be complete
@ browser/client side.

Do not put accept() inside the thread.
i think it is like epoll.

Just "accept" and get away futher processing by
    giving the task to a new thread.

In the above example it is simply a System.out.println(">>>>>>>>");


Each Thread
    |
    +-----1) execution stack FOR JAVA code
    |
    |
    +-----2) execution stack for NATIVE code



Thread stack
    address space

            --> like this many threads


JVM allocates combined stack size of around HALF MB

1) -Xss to change
2) Or with Thread constructor
```

