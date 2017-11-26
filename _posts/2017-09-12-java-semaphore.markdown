---
layout: post
title:  "Understanding Semaphore"
date:   2017-09-12 11:45:46 +0900
categories: Java Semaphore
---

//TODO ichm download and javadoc download

**take** send the signal

**release** receive the signal

types  
[https://en.wikipedia.org/wiki/Semaphore_(programming)](Reference)  
    1) Counting semaphore  
    2) Binary semaphore  


```java
// binary Semaphore
public class MySemaphore {
	private boolean signal;

	public synchronized void take() {
		signal = true;
		notify();
	}
	public synchronized void release() throws InterruptedException {
		while (!signal) {
			wait();
		}
		signal = false;
	}
}
//------------------------------------------
// counting semaphore
public class CountingSemaphore {
	private int cnt;

	public synchronized void take() {
		cnt++;
		notify();
	}

	public synchronized void release() throws InterruptedException {
		while (cnt == 0) {
			wait();
		}
		cnt--;
	}
}

//-----------------------------------------
// bounded semaphore

public class BoundedSemaphore {
	private enum BOUND_LIMIT {
		MAX(10);
		private int x;
		
		private BOUND_LIMIT(final int x) {
			this.x = x;
		}
		public int getValue() {
			return this.x;
		}
	}
	private int cnt;
	public synchronized void take() throws InterruptedException {
		while(BOUND_LIMIT.MAX.getValue()==cnt) wait();
		cnt++;
		notify();
	}
	public synchronized void release() throws InterruptedException {
		while (cnt == 0) {
			wait();
		}
		cnt--;
	}
}
//-----------------
Set the MAX Bound = 1, for lock

semaphore.take(); // cnt becomes 1, next call to this line has to wait
                  // until release() is called.

try {
    // do the work
} finally {
    semaphore.release();
}



```
// TODO  
[http://blog.teamlazerbeez.com/2009/04/20/javas-semaphore-resizing/](ref)


[https://stackoverflow.com/search?q=%5Bjava%5Dsemaphore](ref)

---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/