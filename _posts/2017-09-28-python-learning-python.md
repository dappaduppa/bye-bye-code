---
layout: post
title:  "Learning Python"
date:   2017-09-28 11:50:46 +0900
categories: python, Learning python
---
<style>
table{
    border-collapse: collapse;
    border-spacing: 0;
    border:2px solid #ff0000;
}

th{
    border:2px solid #000000;
}

td{
    border:1px solid #000000;
}
</style>

**easter egg**

```python
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
>>>
```



```python
# clear screen  
import os
os.system('clear')

#funny execution with sis
>>> from os import system as sis
>>>
>>>
>>> sis('clear')
0



# clear screen,  but cursor is on the bottom of the screen  
print("\n"*100)

# python as script
#DhanasearansAir:pythonWS dhanveer$ cat pyscript.py
#!/usr/local/bin/python3
print("this is working")

#input trick
#writing at the end of the script 
input() # to avoid exiting the console
raw_input() # in python 2.x


```

---

**input(), raw_input()** differences  

python2 | python3 | description 
------- | ------- | -----------
input() | eval(input()) | empty input '' string throws **error**
raw_input() | input() | empty input '' string throws no error  

  
---

**__pycache__**

__pycache__ dir are created only for import modules.  
NOT created for programs running in command prompt. (actually dont understand this)

```python
import collections
collections.__cached__
'/usr/local/Cellar/python3/3.6.1/Frameworks/Python.framework/Versions/3.6/lib/python3.6/collections/__pycache__/__init__.cpython-36.pyc'

```

---

**import vs from**

* used by reload  (refer page 68)  
* if name does not change use, import module.attribute  

---

```python
#myfile.py ( note __pycache__ is created)
title='this is fun'

# in the command prompt
>>> import myfile
>>> myfile.title
'this is fun'
>>>

# update the contents of myfile with 
# title='ithis is fun'
>>>
>>> myfile.title
'this is fun' # NOT refelecting ??? need to **reload**
>>>
>>>

>>> import imp
>>> imp.reload(myfile) # reloads are NOT **transitive** 
# i.e. it will NOT import the modules included in the reload.(nesting is NOT avl. for reload)
# have to reload each and every module respectively.
<module 'myfile' from '/Users/dhanveer/workspace/pythonWS/myfile.py'>
>>> myfile.title
'ithis is fun'
>>>
>>> dir(myfile) # lists the attributes
['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'title']
>>>

#exec
>>> exec(open("myfile.py").read())
>>> title
'ithis is fun'

#2.x better ,hmm yeah
execfile('myfile.py') # less verbose

```

**sys.path**

```python
>>> sys.path
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'sys' is not defined
>>>
>>>
>>> import sys
>>> sys.path
['', '/usr/local/Cellar/python3/3.6.1/Frameworks/Python.framework/Versions/3.6/lib/python36.zip', '/usr/local/Cellar/python3/3.6.1/Frameworks/Python.framework/Versions/3.6/lib/python3.6', '/usr/local/Cellar/python3/3.6.1/Frameworks/Python.framework/Versions/3.6/lib/python3.6/lib-dynload', '/usr/local/lib/python3.6/site-packages']
>>>
>>>
>>>
>>>
Dhanasekarans-MacBook-Air:pythonWS dhanveer$ echo $PYTHONPATH

Dhanasekarans-MacBook-Air:pythonWS dhanveer$
Dhanasekarans-MacBook-Air:pythonWS dhanveer$
Dhanasekarans-MacBook-Air:pythonWS dhanveer$ export PYTHONPATH=$PYTHONPATH:my/dummy/path/
Dhanasekarans-MacBook-Air:pythonWS dhanveer$ echo $PYTHONPATH
:my/dummy/path/
Dhanasekarans-MacBook-Air:pythonWS dhanveer$
Dhanasekarans-MacBook-Air:pythonWS dhanveer$
Dhanasekarans-MacBook-Air:pythonWS dhanveer$ python3
Python 3.6.1 (default, Apr 11 2017, 00:34:32)
[GCC 4.2.1 Compatible Apple LLVM 7.3.0 (clang-703.0.31)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
>>> import sys
>>> sys.path
['', '/Users/dhanveer/workspace/pythonWS', '/Users/dhanveer/workspace/pythonWS/my/dummy/path', '/usr/local/Cellar/python3/3.6.1/Frameworks/Python.framework/Versions/3.6/lib/python36.zip', '/usr/local/Cellar/python3/3.6.1/Frameworks/Python.framework/Versions/3.6/lib/python3.6', '/usr/local/Cellar/python3/3.6.1/Frameworks/Python.framework/Versions/3.6/lib/python3.6/lib-dynload', '/usr/local/lib/python3.6/site-packages']
>>>

```

Chapter : Exception handling:
-----------------------------

>>> try:
...     file = open("this is fun")
... except IOError:
...     print "this is fun file NOT found"
...
this is fun file NOT found

================================================================================

import modules with exception usage:
--------------------------------------


try:
    import abcd
except ImportError:
    print "module abcd NOT found..."

>>> try:
...     import abcd
... except ImportError:
...     print "module abcd NOT found..."
...
module abcd NOT found...
>>>


Useful file methods.

file.open()
file.tell()
file.seek(n, 0/1/2)
file.read(n)
file.close()
file.closed --> True / False

    seek / tell / read on the closed file throws ValueError




---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
