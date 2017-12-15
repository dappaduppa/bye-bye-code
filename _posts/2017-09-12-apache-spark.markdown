---
layout: post
title:  "Spark!"
date:   2017-09-12 23:15:46 +0900
categories: Big data, Spark!
---

**RDD**
```
Resilient
Distributed
Dataset
```

## Resilient
```
resilient
rɪˈzɪlɪənt/Submit
adjective
1.
(of a person or animal) able to withstand 
or recover quickly from difficult conditions.

"babies are generally far more resilient than 
new parents realize"

synonyms:	strong, tough, hardy; More

2.
(of a substance or object) able to recoil 
or spring back into shape after bending, stretching, or being compressed.

"a shoe with resilient cushioning"

synonyms:	flexible, pliable, pliant, supple, plastic, elastic, springy, rubbery; More

# Origin
mid 17th century: 
    from Latin resilient- ‘leaping back’, 
    from the verb resilire (see resile).
```


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
## Summary of program execution
![My helpful screenshot]({{ "/assets/spark_flow.jpg" | absolute_url }})


// TODO d3js

## sh
```sh
$ wc README.md
     103     495    3809 README.md
$ head -1 README.md
# Apache　Spark
```
## Apache Spark

-----
### pyspark (python)
```python
>>> lines = sc.textFile("README.md")
>>> lines.count()
103
>>> lines.first()
u'# Apache Spark'

# here , lines=RDD
# various operations can be done on RDD
# ex. lines.count(), lines.first(

# python filter

pythonLines = lines.filter(lambda line : "Python" in line)


```

### spark-shell (scala)
```java
// Note : "var" is the only addition when compared to python.
scala> var lines = sc.textFile("README.md")
lines: org.apache.spark.rdd.RDD[String] = README.md MapPartitionsRDD[1] at textFile at <console>:24

scala> lines.count()
res0: Long = 103

scala> lines.first()
res1: String = # Apache Spark

# scala filter
scala> var pythonLines = lines.filter(line => line.contains("Python"))
pythonLines: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[2] at filter at <console>:26

scala> pythonLines.first()
res2: String = high-level APIs in Scala, Java, Python, and R, and an optimized engine that

scala> pythonLines.count()
res3: Long = 3

# find using grep
$ grep 'Python' README.md
high-level APIs in Scala, Java, Python, and R, and an optimized engine that
## Interactive Python Shell
Alternatively, if you prefer Python, you can use the Python shell:

```

---

## Passing function to Spark INSTEAD OF lambda(python) or =>(scala) syntax
```python
def hasPython(line):
    return "Python" in line

pythonLines = lines.filter(hasPython)
```
```java
JavaRDD<String> pythonLines = lines.filter(
    new Function<String, Boolean>() {
        Boolean call(String line) { return line.contains("Python"); }
    }
);
// Java 8
JavaRDD<String> pythonLines = lines.filter(line -> line.contains("Python"));
```

## Spark concepts
```
Driver program = shell(in prev. example)

# filter 
    also parallelize across cluster

# standalone vs shell
standalone = need to initialize "sc"
shell = "sc" is implicit

# building 
## java / scala
mvn dependency : "spark-core" artifact
https://mvnrepository.com/artifact/org.apache.spark/spark-core

-----
    <groupId>org.apache.spark</groupId>
    <artifactId>spark-core_2.11</artifactId>
    <version>2.2.0</version>
-----
Also can be built by using
scala sbt (scala build tool)
    or
Gradle
tool to interact with maven 

# python
bin/spark-submit includes the dependencies

```

## PyCharm Spark Setup
```
Refer
https://stackoverflow.com/questions/34685905/how-to-link-pycharm-with-pyspark

Imp: set "Show paths for the selected interpreter" ( 9th comment )
```

## Samples Program
---
```
1) Cluster URL = local ( spl. value)
* Runs Spark on one thread
* runs in local m/c
* Not connected to cluster

2) AppName = identify app if connected to cluster 


```
### scala
```scala
import org.apache.spark.SparkConf
import org.apache.spark.SparkContext
import org.apache.spark.SparkContext._
val conf = new SparkConf().setMaster("local").setAppName("My App")
val sc = new SparkContext(conf)
```
### python
```python
from pyspark import SparkConf, SparkContext
conf = SparkConf().setMaster("local").setAppName("My App")
sc = SparkContext(conf = conf)
```
### java
```java
import org.apache.spark.SparkConf;
import org.apache.spark.api.java.JavaSparkContext;
SparkConf conf = new SparkConf().setMaster("local").setAppName("My App");
JavaSparkContext sc = new JavaSparkContext(conf);
```
---

// TODO wordcount example
```java
//Java Word Count
FlatMapFunction
mapToPair
reduceByKey
```

//TODO
## sbt for WordCount
## maven for WordCount


## RDD
```
* RDD split into "partitions"
* computed in diff. cluster > nodes.
* 2 ways to create RDD
    1) Load ext. DataSet
    2) distribute collection objects in the driver PG.

filter() => transformation
first()  => action
```

## RDDs are recomputed on "action"
```python
>>> lines = sc.textFile("README.md")
>>> pyLines = lines.filter(lambda line: "ython" in line)
>>> pyLines.first()
u'high-level APIs in Scala, Java, Python, and R, and an optimized engine that'
>>>
# now edit the "README.md" using vim ,
# and include the line "python fun" at the beginning, then run "first()" action.
>>> pyLines.first()
u'python fun'
# see, the RDD (pyLines) is recomputed, on action "first".

# if you want to persist(dont want to recompute), use RDD.persist()
>>> pyLines = pyLines.persist()

# with the default storage level ( what does that mean???)
# cache() = persist()
```

## Load RDD using collection
```python
>>> inlineLines = sc.parallelize(["this", "is", "fun"])
>>>
>>> inlineLines
ParallelCollectionRDD[12] at parallelize at PythonRDD.scala:480
>>> inlineLines.first()
'this'
>>> inlineLines.count()
3
>>>
```
## transformation Vs action
```
    *         transformations return RDDs, 
    * whereas actions return some other data type.
```
page 28 /  46/274 


---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/