---
layout: post
title:  "Learning Python book Part 1"
date:   2016-07-28 11:50:46 +0900
categories: python, Learning python
---

```python
^Z # Use Ctrl-D (on Unix) or Ctrl-Z (on Windows) to exit

Dhanasekarans-MacBook-Air:~ dhanveer$ python
Python 2.7.10 (default, Oct 23 2015, 19:19:21)
[GCC 4.2.1 Compatible Apple LLVM 7.0.0 (clang-700.0.59.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> #^D
>>> ^D
Dhanasekarans-MacBook-Air:~ dhanveer$ python
Python 2.7.10 (default, Oct 23 2015, 19:19:21)
[GCC 4.2.1 Compatible Apple LLVM 7.0.0 (clang-700.0.59.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> # ^Z
>>>
[3]+  Stopped                 python
Dhanasekarans-MacBook-Air:~ dhanveer$

```

## Current dir
```python
Dhanasekarans-MacBook-Air:~ dhanveer$ python
Python 2.7.10 (default, Oct 23 2015, 19:19:21)
[GCC 4.2.1 Compatible Apple LLVM 7.0.0 (clang-700.0.59.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import os
>>> os.getcwd()
'/Users/dhanveer'
```
## platform
```python
>>> import sys
>>> sys.platform
'darwin'
```

## module reload
```python
>>> import imp
# 3.x use 
# import importlib
# importlib.import_module()
>>> imp.reload('das')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: reload() argument must be module
>>> imp.reload(os)
<module 'os' from '/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/os.pyc'>

# another way
>>> from imp import reload
>>>
>>> reload(os)
<module 'os' from '/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/os.pyc'>

```
## caller ( call other script)
```python
## 2.x and 3.x
exec(open('hw.py').read())

#2.x only
>>> execfile('hw.py')
ehllow
>>> exec(open('hw.py'))
ehllow
```
## string (repeat with new line)
```python
>>> print('ilvu\n' *2)
ilvu
ilvu

>>> 'ilvu\n' * 2
'ilvu\nilvu\n'
```

## print multiple vars
```python
a = 'this'
b = 'is'
c = 'fun'
print(a, b, c) # this is fun
# print a, b, c

```

## module

```python
% python
>>> import threenames # Grab the whole module: it runs here
dead parrot sketch
>>>
>>> threenames.b, threenames.c # Access its attributes
('parrot', 'sketch')
>>>
>>> from threenames import a, b, c # Copy multiple names out
>>> b, c
('parrot', 'sketch'

# dir -> fetch list of all names avl.
>>> dir(threenames)
['__builtins__', '__doc__', '__file__', '__name__', '__package__', 'a', 'b', 'c']


```


---------------------------

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
