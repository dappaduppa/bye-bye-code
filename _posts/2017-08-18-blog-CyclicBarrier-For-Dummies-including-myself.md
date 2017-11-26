---
layout: post
title:  "Trip using CyclicBarrier!"
date:   2017-08-18 11:52:46 +0900
categories: Java, Blog, Concurrency
---

I would like NOT to waste others time, including me.
Straight to the subject. Yes?

**What is CyclicBarrier , rather how to understand CyclicBarrier.**

Story: 
    We(neighbours) used to go on weekend trip, each family owning a car. But the point is
on saturday morning each family ( total 5 families) has to meet at a common starting point.
After everybody assemble, we all start for a trip at once.

I thought this is the perfect use case of CyclicBarrier

```Java
//In java code:
//1) 
// all assemble at a common place, 
// till then 

//  everybody has to await, and can not start the trip until every family arrives.
    cb.await(); // wait for all has to arrive

//2) 
// after everybody assembled, start the trip, But wait ,
// make sure fuel is sufficient or fill it. 

final CyclicBarrier cb = new CyclicBarrier(5, new Runnable(){  // 5 => for 5 families
    // fill petrol
});

//3) start the trip
// this is where the code runs after
// cb.await()
// Each family car on their own speed !!!

```





---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
