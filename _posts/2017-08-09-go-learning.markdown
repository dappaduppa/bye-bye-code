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



Design of Array
following qns and ans. to be thought.

1) fixed or var size
2) does contain size of arr.
3) how is multi dim. array look?
4) empty arr have meaning?

.///////

Ans to the above questions will show

    * Array is

        1) feature (just part of the language)
                Or
        2) in the core design of lang.


Slice
    ----> piece of an array.
        1) fixed size arr.
            built on
        2) flexi, extensible data structure.



slice when passed as argument it is the slice header passed

bytes := [256]byte

slice := bytes[1, 10]

slicePos := bytes.indexRune(slice, '/')

    // here when "slice" argument is mentioned ,

## how to pass array to function.
```go
i can pass slice to function, but I can not pass array to function.

it throws following error
    \arr1_1.go:21: cannot use slice (type [20]byte)
                as type []byte in argument to AddOne
```
## loop idioms
```go
1) two idioms looping through int array
    // intr => integer array
    intr := []int{ 1, 2, 3}

a)

    for v := range intr {
        sum += intr[v]
    }

b)
    for _, v := range intr {
        sum += v
    }
```

## Separate calls
```go
func main() {
     x := make(chan int)

    go func() {
        x <- 1
    }()

     go func() {
        x <- 2
    }()


    fmt.Printf("%d %d\n", <-x, <-x)  // 2 1

}
```

## same call
```go
func main() {
     x := make(chan int)

    go func() {
        x <- 1
        x <- 2
    }()

    fmt.Printf("%d %d\n", <-x, <-x)  // 1 2

}
```

## go notes
```go
var a int = 4

short hand form

a := 4


/******************************************************************************/


channels


str1 := make(chan string)

channels must be assigned properly before getting the value from the channel.

Otherwise deadlock will occcur.

refer C:\dhans\src\go\mychn1.go

/******************************************************************************/

constants

const c1

```

## go notes
```go
go:
---

fmt.Sprintf
    will format the string instead of printing.

java: java.util.Formatter
```

## godoc
```go
godoc fmt Println
    => o/p the doc about fmt.Println

godoc fmt
    => o/p entire doc of "fmt"



$ godoc fmt Println
func Println(a ...interface{}) (n int, err error)
    Println formats using the default formats for its operands and writes to
    standard output. Spaces are always added between operands and a newline
    is appended. It returns the number of bytes written and any write error
    encountered.
    
uint8       int8
uint16      int16
uint32      int32
uint64      int64


byte = uint8
rune = int32


m/c dependant type
1) uint
2) int
3) uintptr


```
boolean value (named after George Boole)


---------------------------

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
