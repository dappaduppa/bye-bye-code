---
layout: post
title: "Java threads" 
date:   2017-11-02 21:25:46 +0900
categories: java, threads, batch
---

Java 8 
*   CompletableFuture ( Future )
*   StampedLock ( ReadWriteLock )
*   Adders & Accumulators
*   ConcurrentHashMap ( new methods added )
*   Single Fork-Join thread pool ( used by streams and CompletableFuture )
*   Common pool ( differs from Fork-Join pool)
*   @Contended ( to avoid false sharing )   


**Future**  

    ExecutorService.submit(Callable/Runnable).get()

**CompletableFuture**  

    CompletableFuture  
        .supplyAsync(Supplier)  
        .whenComplete()    
        .thenApply(Function)
        .thenAccept(Consumer)  
        .thenRun(Runnable)



---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/