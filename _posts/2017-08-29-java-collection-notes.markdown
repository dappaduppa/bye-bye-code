---
layout: post
title:  "My Java Collection Notes"
date:   2017-08-29 17:28:46 +0900
categories: Java
---

**Collection**   
	
    extends Iterable  
			
            iterator()
			
	
	add
	addAll
	remove
	removeAll
	contains
	containsAll
	retainAll
	clear
	size
	isEmpty
	iterator
	toArray
	toArray T
	equals
	hashCode

-----

**Queue**

symbols indicate similar operation  

`=`   
`==`  
`===`  

    extends

        Collection

    throws exception:

    add      =
    element  ==
    remove   === ( from Collection )
            ///////
            not throwing exception
    offer    =
    peek     ==
    poll     ===

* bounded = restrict the no. of elements in queue.
* NOT necessarily FIFO.
* java.util.concurrent queue : some of them are bounded.  
    -> offer return false  
    -> add throws IllegalStateException  
* java.util queues are NOT bounded.


**Sample:**
    Assume queue is empty.    
    1) remove() -> throws NoSuchElementException  
    2) poll()   -> return null

null:  
    Intention was NOT to allow null insertion.  
    But LinkedList retrofitted to implement Queue....and the story so on.   
    So null allowed. but avoid it. since peek(), poll() has spl. meaning of null return value.  

equals() and hashCode():
    use the default identity based version from Object.
    

**Queue Implementation:**

1) PriorityQueue
```java
final Queue<String> queue = new PriorityQueue<>(String.CASE_INSENSITIVE_ORDER);
queue.addAll(Arrays.asList("one", "Two", "three"));
```
Queue order:  
one  
three  
Two  


    2) LinkedList


**Deque**
    Double ended queue

    Deque:	pronounced as "deck" : double ended queue.
	extends Queue
	
	addFirst		offerFirst
	addLast			offerLast
	
	removeFirst		pollFirst
	removeLast		pollLast
	
	getFirst		peekFirst
	getLast			peekLast

---------------------------

Deque implementation:  
1) ArrayDeque  
2) LinkedList  

LinkedList vs ArrayDeque :   
    LinkedList maintains the link of next element.
[Reference](https://stackoverflow.com/questions/6163166/why-is-arraydeque-better-than-linkedlist)

LinkedList vs ArrayList
[Reference](https://stackoverflow.com/questions/322715/when-to-use-linkedlist-over-arraylist)

**LinkedList** is Queue and Deque  

---

**streams**

[Reference](http://winterbe.com/posts/2014/03/16/java-8-tutorial/)  
[Java 8 features reference](http://winterbe.com/posts/2014/03/16/java-8-tutorial/)  

**Deafult method(Extension methods)**

```java
public interface MyJava8Interface {
	void abcd();
	default void abcd1() {
		System.out.println(">>>>");
	}
}
```

**Two ways of sorting(by descending)**

```java
public class MySampleStreamer {
	public static void main(String[] args) {
		final List<String> list = Arrays.asList("one", "two", "three", "four");

        // using streams
		List<String> list2 = list.stream().sorted(Comparator.comparing(String::toString).reversed()).collect(Collectors.toList());
		System.out.println(list2);
		
        //using functinal interface
		Collections.sort(list, (a, b) -> b.compareTo(a));
		System.out.println(list);
	}
}
```



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/