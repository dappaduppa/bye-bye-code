---
layout: post
title:  "Python for data analysis(book) Pandas"
date:   2017-10-17 21:56:46 +0900
categories: Python, Pandas
---

**string loading using json**

```python
import json

line = '{ "a" : "this is \/fun"}'
jsondata = json.loads(line) # line should be string, NOT dict. So single quote around the "line" is required
print(jsondata)
    
    {'a': 'this is /fun'}

```

**loading keys from json as list**

```python
import json 
lines = [ '{ "a": "Mozilla\/5.0 (Windows NT 6.1; WOW64) AppleWebKit\/535.11 (KHTML, like Gecko) Chrome\/17.0.963.78 Safari\/535.11", "c": "US", "nk": 1, "tz": "America\/New_York", "gr": "MA", "g": "A6qOVH", "h": "wfLQtf", "l": "orofrog", "al": "en-US,en;q=0.8", "hh": "1.usa.gov", "r": "http:\/\/www.facebook.com\/l\/7AQEFzjSi\/1.usa.gov\/wfLQtf", "u": "http:\/\/www.ncbi.nlm.nih.gov\/pubmed\/22415991", "t": 1331923247, "hc": 1331822918, "cy": "Danvers", "ll": [ 42.576698, -70.954903 ] }' ]

records = [ json.loads(line) for line in lines ]
list = [ rec['tz'] for rec in records if 'tz' in rec ]

```

**loads vs dumps**

loads = return dict
dumps = return str

```python
>>> import json
>>>
>>> line1 = '{ "yes": "is", "this" : "fun" }'
>>>
>>> line1
'{ "yes": "is", "this" : "fun" }'
>>>
>>> data = json.loads(line1)
>>>
>>> data
{'yes': 'is', 'this': 'fun'}
>>>
>>> type(data)
<class 'dict'>
>>>
>>> data = json.dumps(line1)
>>> type(data)
<class 'str'>
```

---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
