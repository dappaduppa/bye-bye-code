---
layout: post
title:  "Zero turnaround article"
date:   2017-08-08 11:45:46 +0900
categories: Java Lamba, Streams
---

**Lambas**  


primitives : int, double, ...  
Boxed primitives : Integer, Double  

1) Boxed primitives Non functional value : null  

```java
    // Broken comparator - can you spot the flaw?
    Comparator<Integer> naturalOrder = new Comparator<Integer>() {
        public int compare(Integer first, Integer second) {
            return first < second ? -1 : (first == second ? 0 : 1);
        }
    };
```

**2) Checking == on boxed primitives is always wrong( sample 1. also )**  
     **Sample 1. == evaluate to object ref comparison**  

```java
// throws NPE , why?
public class Unbelievable {
    static Integer i;

    public static void main(String[] args) {
        System.out.println(i==42);
    }
}
```


---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/