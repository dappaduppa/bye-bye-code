---
layout: post
title:  "Learning Python book Part 2"
date:   2016-07-29 11:50:46 +0900
categories: python, Learning python
---

## Core data types
```python
object type | example literals/creation
----------- | -------------------------
Numbers     | 1234, 3.1415, 3+4j, 0b111, Decimal(), Fraction()
Strings     | 'spam', "Bob's", b'a\x01c', u'sp\xc4m'
Lists       | [1, [2, 'three'], 4.5], list(range(10))
Dictionaries| {'food': 'spam', 'taste': 'yum'}, dict(hours=10)
Tuples      | (1, 'spam', 4, 'U'), tuple('spam'), namedtuple
Files       | open('eggs.txt'), open(r'C:\ham.bin', 'wb')
Sets        | set('abc'), {'a', 'b', 'c'}

>>> tuple('samp')
('s', 'a', 'm', 'p')

>>> list(range(10))
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

>>> len(str(2 ** 1000000)) # How many digits in a really BIG number?
301030

>>> 22/7
3
>>> 22.0/7
3.142857142857143
```

## use "print" to avoid seeing odd values

```python
>>> 3.1415 * 2 # repr: as code (Pythons < 2.7 and 3.1)
6.2830000000000004
>>> print(3.1415 * 2) # str: user-friendly
6.283
```


---------------------------

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
