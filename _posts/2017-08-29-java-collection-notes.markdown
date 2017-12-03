---
layout: post
title:  "My Java Collection Notes"
date:   2017-08-29 17:28:46 +0900
categories: Java, Collection
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

**print collection**
```java
void printCollection(Collection<?> c) {
    for (Object e : c) {
        System.out.println(e);
    }
}

    vs

// Compliation error
// Type mismatch: cannot convert from element type capture#1-of ? to String	
void printCollection(Collection<?> c) {
    for (String e : c) {
        System.out.println(e);
    }
}
```

**casting in collection**
```java 
Object[] oa = new Object[100];

String[] sa = new String[100];

oa = sa;

sa = oa; // Not OK. explicit cast required.

sa = (String[])oa; // Compile ok, but
                   // ClassCastException at runtime.


------------------
    Object[] oa = new Object[100];
    Collection<Object> co = new ArrayList<Object>();


    fromArrayToCollection(oa, co);

    String[] sa = new String[100];
    Collection<String> cs = new ArrayList<String>();

    fromArrayToCollection(sa, cs);

    fromArrayToCollection(sa, co); // T is inferred as Object

    static <T> void fromArrayToCollection(T[] a, Collection<T> c) {
        for (T t : a) {
            c.add(t);
        }
    }
```

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
	
	a) priority queue is sorted queue
	b) btw, LinkedList is queue!
	c) java.util.Queue s are NOT bounded
	d) some of java.util.concurrent queue are bounded.( capacity restriction )
		add throws IllegalStateException
		offer returns false	if capacity is exceeded.	
	e) EvictingQueue (guava)
	   size limiting but elements are added ( and the head element is removed when the size full)

## EvictingQueue

```java
	import java.util.Queue;
	import com.google.common.collect.EvictingQueue;

	Queue<Integer> fifo = EvictingQueue.create(2); 
	fifo.add(1); 
	fifo.add(2); 
	fifo.add(3); 
	System.out.println(fifo); 

	// Observe the result: 
	// [2, 3]
```

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

## comparator
```java
	// Collections.sort vs list.sort() 
	// TODO 
	// https://stackoverflow.com/questions/24436871/very-confused-by-java-8-comparator-type-inference

	Comparator.comparing((Person p)->p.firstName)
			.thenComparing(p->p.lastName)
			.thenComparingInt(p->p.age);

	If you have accessor methods:
	Comparator.comparing(Person::getFirstName)
			.thenComparing(Person::getLastName)
			.thenComparingInt(Person::getAge);
			
```

//TODO

https://www.google.co.in/#q=collections.sort+million+record&spf=725

https://www.google.co.in/#q=collections.sort+million+record&spf=725


sort million records and see n redis vs java


		
## Arrays as List:

```java
List<String> list = Arrays.<String>asList("abcd", "efgh");
	// is equivalent to 
	

	List<String> list = Arrays.<String>asList(new String[]{"abcd", "efgh"});
	//--> It does not create List<String[]>

	//	If you want to create List<String[]>			
	List<String> list = Arrays.<String>asList(new String[]{"abcd"}, new String[]{"efgh"});
						
	// NG: this will not work.
	List<String> list = Arrays.<String>asList({"abcd", "efgh"});
	
	// this is ok.
	List<String> list = Arrays.<String>asList(new String[]{"abcd", "efgh"});
	
	//list modification (remove, add ) obtained via asList will throw
	//java.lang.UnsupportedOperationException, since the list is just a wrapper.

```

## toArray vs toArray T :  

```java
// toArray T is preferred...

List<String> list = ....

// NG: ClassCastException ... can not convert Object[] to String[]
String[] strArr = (String[])list.toArray();

// OK
String[] strs = new String[0]; 
//strs.length = 0 ; 
//strs -> has reference NOT null

String[] strArr = list.<String>toArray(strs);
// strs and strArr has different reference.

//if size is equal or greater than the size of the list, 
//strs and strArr has the same reference.
String[] strs = new String[size];
```

## removeAll null element from collection

```java
List<String> list = ...

//NG
list.removeAll(null); 
// --> throws NPE... so code review grep for removeAll(null)

//OK
list.removeAll(Collections.singleton(null));

// -----------
// removeAll api reference
boolean removeAll(Collection<?> c)

// ArrayList removeAll implementation
public boolean removeAll(Collection<?> c) {
	Objects.requireNonNull(c);
	return batchRemove(c, false);
}

// Objects.requireNonNull implementation. What is the use ? ha. 
    public static <T> T requireNonNull(T obj) {
        if (obj == null)
            throw new NullPointerException();
        return obj;
    }
// -----------

//OK
list.remove(null);
// --> removes only one null element ( the very first )

// Also removeAll throws UnsupportedOperationException, 
// if the list is created using Arrays.asList()
```

## remove duplicates:
```java
List<String> list = ...

Set<String> set = new HashSet<>(list); // list order is NOT preserved.

Set<String> set = new LinkedHashSet<>(list); // list order is preserved.

//The original list could have been created using Arrays.asList,
//because new Set is created from the list.... 
//and NO UnsupportedOperationException is thrown., on remove.,etc

```
## Map iteration
```java
Map<String, String> map = ...

for(Map.Entry<String, String> entry : map.entrySet()) {
	final String key = entry.getKey();
	final String value = entry.getValue();
}
```

//TODO : 
// why abstractlist remove always throw unsupportedoperationexception:  
// how to view the trace path of all the files, methods executed... in java



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/