---
layout: post
title:  "find existence of val. in int array using java !"
date:   2017-08-03 22:40:46 +0900
categories: leetcode, qn1
---


```java
import java.util.ArrayList;
import java.util.List;
import java.util.stream.IntStream;

public class IntValExistInIntArr {

	public static void main(String[] args) {
		
		final int findExistenceOf = 4;
		
		// method 1
		final int[] intArr = { 1, 2, 3, 4, 5 };
		final boolean isFound = IntStream.of(intArr).anyMatch(x -> x == findExistenceOf);
		System.out.println(isFound);
		
		// method 2
		// Arrays.asList(intArr) wont work, since intArr is NOT Integer Array. it is "int" array
		final List<Integer> list = new ArrayList<>(intArr.length);
		for (int i : intArr) {
			list.add(i);
		}
		final boolean isFoundByMethod2 = list.contains(findExistenceOf);
		System.out.println(isFoundByMethod2);
	}

}

```

