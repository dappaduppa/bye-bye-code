---
layout: post
title:  "learning go!"
date:   2017-08-09 11:49:46 +0900
categories: Go
---

* GOPATH : set the installation dir. 
* when passing objects to function, copy is passed.
  to reflect the changes in the function, pass the pointer object.

```go

package main

import "fmt"

type Person struct {
	Name     string
	NickName string
	Salary   int
}

func main() {
	p := fmt.Println

	p1 := Person{Name: "dhanveer", NickName: "mandaya", Salary: 3000000}

	incSalary(&p1)
	p(p1.Salary)

}


func incSalary(per *Person) {
	per.Salary += 1500000	
	fmt.Println(per.Salary)
}

```
* how abt this and why ? 

```go
package main

import "fmt"

type Person struct {
	Name     string
	NickName string
	Salary   int
}

func main() {
	p := fmt.Println

	p1 := Person{Name: "dhanveer", NickName: "mandaya", Salary: 3000000}

	incSalary(&p1)
	p(p1.Salary)

}


func incSalary(per *Person) {

	per = &Person{"doppa", "mokkai", 1000}
	per.Salary += 1500000	
	//fmt.Println(per.Salary)
}
```

* associate method with structure:
    so that method can specify the pointer of the struct as **receiver**
    refer go.pdf, section "Functions on Structures"

* structure can not have constructor, but a function can return instance of object.  
  Also it can return the pointer of the instance of object.  
  

* slice :
	cap  
	len  
	append  

	len is not fixed, it can expand  

	so -ve values in the slicing, use len  

	for ex: except the last one item, slice it !   
```go
	slice1 := slice1[:len(slice1)-1]
```


**Index function (JS) **

```javascript
str = "this is fun"
console.log(str.indexOf(' ', 5)) // after the first 5 chars, find the indexOf ' ' => result 7
```

```go
	str1 := "this is fun"	
	p(strings.Index(str1[5:], " "))
	// result 2, since the original string is sliced already from that index is calculated
```

Go routines:
------------

1) little overhead  
2) M:N threading model  
	M -> Go routine thread  
	N -> OS thread.  
	Mulitple GO routines run in single OS thread.  
	WHY ?: other languages are equipped with this ? I dont know, study later  

listening port using Go pgm  
----------------------------  

[Go listen on port which is already in use not returning error](https://stackoverflow.com/questions/24483955/go-listen-on-port-which-is-already-in-use-not-returning-error)  

[How to use next available port in http.ListenAndServe](https://stackoverflow.com/questions/43424787/how-to-use-next-available-port-in-http-listenandserve)  


[golang: Best way to implement global counters for highly concurrent applications?](https://stackoverflow.com/questions/16783273/golang-best-way-to-implement-global-counters-for-highly-concurrent-applications)


---------------------------

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
