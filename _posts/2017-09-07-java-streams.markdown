---
layout: post
title:  "Java Stream Notes"
date:   2017-09-07 11:45:46 +0900
categories: Java Streams
---

[Reference streams](http://winterbe.com/posts/2014/03/16/java-8-tutorial/)

**Java Streams**

    1) Intermediate ( eg. filter)
    2) terminal ( eg. ForEach)

    * operate on List, Set
    * NOT on map

    1) Collection.stream()  
    2) Collection.parallelStream()  

**Sample**

```java
final List<String> list = new ArrayList<>();
list.add("one");
list.add("two");
list.add("three");

list
.stream() // normal(stream) or parallelStream() ?
.filter(str -> !str.startsWith("o")) // intermediate (filter)
.forEach(System.out::println); // terminal (forEach)

// more intermediate
list
.stream() // normal(stream) or parallelStream() ?
.filter(str -> !str.startsWith("o")) // intermediate (filter)
.sorted() // another intermediate ( sort after filter, ofcourse based on business requirement)
// sorted() will NOT change the original list.
.forEach(System.out::println); // terminal (forEach)


// intermediate conversion using map
list
.stream() // normal(stream) or parallelStream() ?
.map(String::toUpperCase) // intermediate conversion
.forEach(System.out::println);;

// match operation(terminal)
//  1) anyMatch
//  2) allMatch
//  3) NoneMatch
final boolean foundO = list
.stream() // normal(stream) or parallelStream() ?
.anyMatch(str -> !str.startsWith("o")); // terminal

System.out.println(foundO);

//count(terminal operation)
final long cnt = list
.stream() // normal(stream) or parallelStream() ?
.count(); // count (terminal)
System.out.println(cnt);


// Optional and reduce 

list.stream()
.forEach(System.out::print); // onetwothree
// forEach((s)->System.out.println(s + " ")); // one two three  

System.out.println();
System.out.println(">>>>>");

Optional<String> opt = 
        list.stream()
        .reduce((s1, s2) -> s1 + " " + s2);
opt.ifPresent(System.out::println); // one two three


```    

**Stream vs parallel stream**
```java
package effjava;

import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

enum MyMax {
	MAX(1000000);

	private final int val;

	private MyMax(int val) {
		this.val = val;
	}

	public int getVal() {
		return this.val;
	}

}

public class MyJavaStreamAPI {

	public void test1(final List<UUID> list) {
		final long start = System.nanoTime();
		list.stream().sorted();
		final long end = System.nanoTime();

		System.out.println(end - start);
	}

	public void test2(final List<UUID> list) {
		final long start = System.nanoTime();
		list.parallelStream().sorted();
		final long end = System.nanoTime();

		System.out.println(end - start);
	}

	public static void main(String[] args) {

		List<UUID> list = new ArrayList<>();
		for (int i = 0; i < MyMax.MAX.getVal(); i++) {
			list.add(UUID.randomUUID());
		}
		
		final MyJavaStreamAPI thisOb = new MyJavaStreamAPI();
		thisOb.test1(list); // 2272366
		thisOb.test2(list); // 11808

	}
}

```

**Map**

	1) Map supports 
		forEach
		computeIfAbsent
		computeIfPresent

```java
final Map<Integer, String> map = new HashMap<>();
for (int i = 0; i < MyMax2.MAX.getVal(); i++) {
	map.putIfAbsent(i, i + "");
}

map.forEach((k, v) -> System.out.println(k + "#" + v));

System.out.println(">>>>>>>");

map.computeIfPresent(3, (num, val) -> "doppa"); // key=3, val="doppa"
map.computeIfPresent(4, (num, val) -> null); // key 4 is removed 
// TODO how to assign "null" to key

// since 1.8 version
map.remove(3, "dappa"); // remove only if key=3 is assigned to "dappa"
map.get(3); // "doppa" ( in the prev. step, key is NOT removed, 
// since 3 is assigned to "doppa", not "dappa")

map.forEach((k, v) -> System.out.println(k + "#" + v));

// getOrDefault
map.getOrDefault(42, "not found"); // "not found" is returned, since key=42 NOT found

//merge
map.merge(1, "one", (val,newVal) -> val.concat(newVal));
map.get(1); // 1one

	//merge ( note the order of "concat" is important!)
	map.merge(1, "one", (val,newVal) -> newVal.concat(val));
	map.get(1); // one1

```

## Samples
```java
List<Transaction> groceryTransactions = new Arraylist<>();
for(Transaction t: transactions){
  if(t.getType() == Transaction.GROCERY){
    groceryTransactions.add(t);
  }
}
Collections.sort(groceryTransactions, new Comparator(){
  public int compare(Transaction t1, Transaction t2){
    return t2.getValue().compareTo(t1.getValue());
  }
});
List<Integer> transactionIds = new ArrayList<>();
for(Transaction t: groceryTransactions){
  transactionsIds.add(t.getId());
}
------------>
List<Integer> transactionsIds = 
    transactions.stream()
                .filter(t -> t.getType() == Transaction.GROCERY)
                .sorted(comparing(Transaction::getValue).reversed())
                .map(Transaction::getId)
                .collect(toList());
short circuit : Limit
-------------------------------------------------------------------------
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
List<Integer> twoEvenSquares = 
    numbers.stream()
           .filter(n -> {
                    System.out.println("filtering " + n); 
                    return n % 2 == 0;
                  })
           .map(n -> {
                    System.out.println("mapping " + n);
                    return n * n;
                  })
           .limit(2)
           .collect(toList());
filtering 1
filtering 2
mapping 2
filtering 3
filtering 4
mapping 4
----
----
BeerDrinkers
Person
	age
	
List<Person> beerDrinkers = persons.stream()
 .filter(p -> p.getAge() > 18).collect(Collectors.toList())
----
 List<Person> beerDrinkers = persons.stream() 
 .filter(p -> p.getAge() > 16).collect(Collectors.toList()); 
---
Set add returns false on duplicate
---
persons.forEach(p -> p.setLastName("Doe"))
---
List<Person> persons = … 
// sequential version
Stream<Person> stream = persons.stream();
 
//parallel version 
Stream<Person> parallelStream = persons.parallelStream(); 
------
```

## More streams 
```java
Stream<Student> students = persons.stream()
      .filter(p -> p.getAge() > 18)
      .map(new Function<Person, Student>() {
                  @Override
                  public Student apply(Person person) {
                     return new Student(person);
                  }
              });
	(or)
Stream<Student> students = persons.stream()
        .filter(p -> p.getAge() > 18)
        .map(person -> new Student(person));
	(or)

Stream<Student> students = persons.stream()
        .filter(p -> p.getAge() > 18)
        .map(Student::new);

List<Student> students = persons.stream()
        .filter(p -> p.getAge() > 18)
        .map(Student::new)
        .collect(Collectors.toList());

List<Student> students = persons.stream()
        .filter(p -> p.getAge() > 18)
        .map(Student::new)
        .collect(Collectors.toCollection(ArrayList::new));


List<Student> students = persons.stream()
        .parallel()
        .filter(p -> p.getAge() > 18)  // filtering will be performed concurrently
        .sequential()
        .map(Student::new)
        .collect(Collectors.toCollection(ArrayList::new));

```

```java
 // use them in with streams 
 new ArrayList<String>().stream().  
 // peek: debug streams without changes 
 peek(e -> System.out.println(e)).  
 // map: convert every element into something   
 map(e -> e.hashCode()).  
 // filter: pass some elements through 
 filter(e -> ((e.hashCode() % 2) == 0)).    
 // collect the stream into a collection  
 collect(Collectors.toCollection(TreeSet::new))
```

## Optional
```java
 Optional<User> user = Optional.ofNullable(getUser(user));  
 Optional<Location> location = user.map(u -> getLocation(user)); 
```

---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/