---
layout: post
title:  "Spark!"
date:   2017-09-12 23:15:46 +0900
categories: Big data, Spark!
---

**Spark**

https://www.youtube.com/watch?v=9mELEARcxJo  
15:33

**StorageLevel**  
from pyspark import StorageLevel

**filter**  
```python
readmeLines = sc.textFile("README.md")
sparkLines = readmeLines.filter(lambda line: "spark" in line)
sparkLines.first()
```

## Learning Spark

### Chapter 7 (Running on a cluster)
```txt
Cluster Managers:
* YARN
* Mesos
* Standalone (Spark Cluster Manager)
```

## Master Slave architecture
```txt
                                    +----------+
                                    |  Driver  |
                                    +----------+
                                         +
                                         |
                                         |
                                         |
                                         +
                                    Cluster Master(Manager)
                                        YARN
                                        Mesos
                                        Standalone
                              ======+----------+=======
                                    |  Manager | 
                                /   +----------+    \
                            /             |             \
                        /                 |                 \   
        +----------+               +----------+              +----------+
        | Executor |               | Executor |              | Executor |
        +----------+               +----------+              +----------+


Cluster Manager : Launch Spark Application on external m/c.

Driver : 
 * main method runs
 * create sc (SparkContext)
 * create RDD
 * Perform "actions"
 * Perform "transformations"

 Note: spark shell preloaded with "sc"

 * Spark driver compiles user program into "units" (task)
 * create RDDs from input, 
 * derive RDDs using transformations
 * perform "action"s to collect / save data.

 * Direct Acyclic Graph (DAG):
    1) implicitly created by spark program.


```

## Deploying to a Cluster with Spark-Submit
```py
# run locally
bin/spark-submit my_script.py

# spark submit format
bin/spark-submit [options] <app jar | python file> [app options]
#                 ----1---                 
#                          ----------2------------
#                                                  ------3------ 
# submit to "Spark Standalone cluster"
bin/spark-submit --master spark://host:7077 --executor-memory 10g my_script.py
#                ----------------------1-------------------------
#                                                                 ------2-----
# here executor memory size also can be specified.
```

## maven-shade-plugin
```txt
resolve dependencies using maven, 
doing by changing src to different namespace 
```

![My helpful screenshot]({{ "/assets/abcd.png" | absolute_url }})







---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/