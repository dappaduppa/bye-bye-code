---
layout: post
title:  "Java Lambdas"
date:   2016-02-16 08:10:26 +0900
categories: Java, Lamdas
---

http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/Lambda-QuickStart/index.html

Overview:
---------

Java Lambda expressions

	java.util.function
		Predicate
		Function

## Interface with only one method:
```java
"functional interface" 
previously it is called as SAM ( Single Abstract Method type)

---

package java.awt.event;
import java.util.EventListener;

public interface ActionListener extends EventListener {
    public void actionPerformed(ActionEvent e);
}

---

Some more functional interfaces:
    Runnable
    Comparator
```		
		
## Lambda expresssion syntax:

	Argument List Arrow Token Body 
	(int x, int y) -> x + y 
	
	() -> 42
	
	(String str) -> { System.out.println(s); };
		or
	(String str) -> System.out.println(s);
	
## Youtube search "Jump-Starting Lambdas"

```java
stream()
.filter()
.mapToxxx ( Int/Double)
.average()


OptionalDouble

parallelStream()
.filter()
.mapToxxx()
.max()


stream()
.filter()
.collect(Collectors.toList())


.forEach(Person::printWesternName);
```

http://www.lambdafaq.org/what-is-a-lambda-expression/

http://openjdk.java.net/projects/lambda/
