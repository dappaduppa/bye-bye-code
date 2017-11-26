---
layout: post
title:  "Java String"
date:   2017-09-07 16:45:46 +0900
categories: Java String 
---

**String**
```java
// assemble ph no.
final String phNum = String.join("-", "070", "1395", "4324");
System.out.println(phNum); // 070-1395-4324

//split and assemble ph no
// 070-1395-4324
Arrays.asList(phNum.split("-")).stream()
        .reduce((s1, s2) -> s1 + "-" + s2)
        .ifPresent(System.out::println);
```

**String char handling**

```java
// get unique chars from String
final String s1 = "this is fun";
s1.chars().distinct().forEach(s -> System.out.println(s));

System.out.println(">>>>>>>1111>>>>>>");

s1.chars().distinct().mapToObj(c -> String.valueOf(c))
        .forEach(s -> System.out.println(s));

System.out.println(">>>>>>>2222>>>>>>");

// note down the effect of casting (char), because chars() gives IntStream
s1.chars().distinct().mapToObj(c -> String.valueOf((char) c))
        .forEach(s -> System.out.println(s));
/*
Output:

116
104
105
115
32
102
117
110
>>>>>>>1111>>>>>>
116
104
105
115
32
102
117
110
>>>>>>>2222>>>>>>
t
h
i
s
 
f
u
n
*/


```


**String Collectors.joining()**

```java
String abcdStrs = "A	B	C	D	E	F	G	H	I	J	K	L	M	N	O	P	Q	R	S	T	U	V	W	X	Y	Z";
abcdStrs = abcdStrs.replaceAll("\\s+", "");
abcdStrs += abcdStrs + "ds";
System.out.println(abcdStrs);
//ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZds

abcdStrs = abcdStrs.chars().distinct().sorted()
        .mapToObj(c -> String.valueOf((char) c))
        .collect(Collectors.joining());

System.out.println(abcdStrs);
//ABCDEFGHIJKLMNOPQRSTUVWXYZds

```

**Find ph nos. by area code (sample 070-)**

```java
final List<String> list = new ArrayList<>();
list.add("070-1395-4324");
list.add("080-4308-3232");
list.add("070-4208-3526");

final String phNums = list.stream().collect(Collectors.joining(":"));
System.out.println(phNums);
// 070-1395-4324:080-4308-3232:070-4208-3526

// method1
System.out.println(Arrays.asList(phNums.split(":")).stream()
        .filter(s -> s.startsWith("070-"))
        .collect(Collectors.joining(":")));
// 070-1395-4324:070-4208-3526

// method2 (using Pattern)
System.out.println(Pattern.compile(":").splitAsStream(phNums)
        .filter(s -> s.startsWith("070-"))
        .collect(Collectors.joining(":")));
//070-1395-4324:070-4208-3526
```

**Pattern as predicate**
```java
// Pattern.compile(".*@gmail\\.com");
System.out.println(Stream
        .of("duppa1@gmail.com", "dappa1@hotmail.com",
                "doppa@gmail.com", "doppi@gmail.com")
        .filter(Pattern.compile(".*@gmail\\.com").asPredicate())
        .collect(Collectors.joining(" : ")));
// duppa1@gmail.com : doppa@gmail.com : doppi@gmail.com

```

**Stream, Reduce, IfPresent**
```java
final List<String> list = new ArrayList<>();
list.add("070-1395-4324");
list.add("080-4308-3232");
list.add("070-4208-3526");

list.stream().reduce((s1, s2)->s1+":" +s2).ifPresent(System.out::println);
// 070-1395-4324:080-4308-3232:070-4208-3526
```

**Stream, Filter, Reduce, IfPresent**
```java
final List<String> list = new ArrayList<>();
list.add("070-1395-4324");
list.add("080-4308-3232");
list.add("070-4208-3526");

list.stream().filter(s->s.startsWith("070-")).reduce((s1, s2)->s1+":" +s2).ifPresent(System.out::println);
//070-1395-4324:070-4208-3526
```

**Integer MIN_VALUE, MAX_VALUE**
```java
        MIN_VALUE = -2147483648
        MAX_VALUE = 2147483647
```

---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/