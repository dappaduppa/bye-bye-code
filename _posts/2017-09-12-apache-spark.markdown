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



---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/