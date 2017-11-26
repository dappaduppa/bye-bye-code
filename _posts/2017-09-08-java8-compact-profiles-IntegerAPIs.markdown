---
layout: post
title:  "Java Compact profile , Integer APIs"
date:   2017-09-08 12:15:46 +0900
categories: Java Compact Profile, Integer APIs
---

[Reference](http://www.informit.com/articles/article.aspx?p=2216988)

    1) Java 8 --> Compact profile
    2) Java 9 --> Project jigsaw

    subset --> NO ( example andriod , a big problem)

    javac -profile compact1 Abcd.java

    jdeps Abcd.class

    jdeps -verbose -P Abcd.class

```java


import java.sql.Statement;

public class Abcd {
	public static void main(String... args) {
		System.out.println("this is Abcd");
	}
}
```

```bash
#// Just import-ing alone, runtime will not require specific compact profile,
#// but compile time require

// compile time:
DhanasearansAir:src dhanveer$ javac -profile compact1 Abcd.java
Abcd.java:1: error: Statement is not available in profile 'compact1'
import java.sql.Statement;
               ^
1 error
DhanasearansAir:src dhanveer$
DhanasearansAir:src dhanveer$ javac -profile compact2 Abcd.java
DhanasearansAir:src dhanveer$
DhanasearansAir:src dhanveer$
DhanasearansAir:src dhanveer$ javac -profile compact3 Abcd.java
DhanasearansAir:src dhanveer$
DhanasearansAir:src dhanveer$
DhanasearansAir:src dhanveer$ jdeps -verbose -P Abcd.class
Abcd.class -> /Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/jre/lib/rt.jar (compact1)
   Abcd                                               -> java.io.PrintStream                                compact1
   Abcd                                               -> java.lang.Object                                   compact1
   Abcd                                               -> java.lang.String                                   compact1
   Abcd                                               -> java.lang.System                                   compact1

```    
```bash
# When using the import-ed class, runtime(ofcourse) also requires the specific compact profile.

DhanasearansAir:src dhanveer$ cat Abcd.java
import java.sql.Statement;

public class Abcd {
	public static void main(String... args) {
		System.out.println("this is Abcd=" + Statement.class.getClass().getName());
	}
}

DhanasearansAir:src dhanveer$ javac -profile compact3 Abcd.java
DhanasearansAir:src dhanveer$ jdeps -verbose -P Abcd.class
Abcd.class -> /Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/jre/lib/rt.jar (compact2)
   Abcd                                               -> java.io.PrintStream                                compact1
   Abcd                                               -> java.lang.Class                                    compact1
   Abcd                                               -> java.lang.Object                                   compact1
   Abcd                                               -> java.lang.String                                   compact1
   Abcd                                               -> java.lang.StringBuilder                            compact1
   Abcd                                               -> java.lang.System                                   compact1
   Abcd                                               -> java.sql.Statement                                 compact2

```


---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/