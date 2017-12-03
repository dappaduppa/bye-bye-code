---
layout: post
title:  "Regex Notes"
date:   2016-07-07 14:28:46 +0900
categories: Java, Regex
---

## regex metachars
```java
12 characters with special meanings
	 the backslash \
	 the caret ^
	 the dollar sign $
	 the period or dot .
	 the vertical bar or pipe symbol |
	 the question mark ?
	  the asterisk or star * 
	  the plus sign +
	  the opening parenthesis (
	  the closing parenthesis )
	  and the opening square bracket [
	  the opening curly brace {
	  
These special characters are often called "metacharacters".
```

## Escaping in regex
	 split("\\."), 
	 or use character class [] to represent literal character(s) like so split("[.]"), 
	 or use Pattern#quote() to escape the entire string like so split(Pattern.quote(".")).
	 
	 String[] parts = string.split(Pattern.quote(".")); // Split on period.

```java
String string = "004-034556";
String[] parts = string.split("-");
String part1 = parts[0]; // 004
String part2 = parts[1]; // 034556


positive lookaround. (?<=ch) (?=ch)
------------------

String string = "004-034556";
String[] parts = string.split("(?<=-)");
String part1 = parts[0]; // 004-
String part2 = parts[1]; // 034556

String string = "004-034556";
String[] parts = string.split("(?=-)");
String part1 = parts[0]; // 004
String part2 = parts[1]; // -034556
	
	
limiting resulting part
-----------------------

String string = "004-034556-42";
String[] parts = string.split("-", 2);
String part1 = parts[0]; // 004
String part2 = parts[1]; // 034556-42
	
```


## python regex

```python

>>> s = '100 NORTH BROAD ROAD'

>>> s[:-4] + s[-4:]
'100 NORTH BROAD ROAD'

>>> import re
>>> re.sub('ROAD$', 'RD.', s)
'100 NORTH BROAD RD.'

b for boundary

>>> s = '100 ROAD'
>>>
>>> re.sub('\\bROAD$', 'RD.', s)
'100 RD.'
>>> re.sub(r'\bROAD$', 'RD.', s)
'100 RD.'


>>> s = '100 BROAD'
>>> re.sub('\\bROAD$', 'RD.', s)
'100 BROAD.'

>>> re.sub(r'\bROAD$', 'RD.', s)
>>> re.sub(r'\bROAD$', 'RD.', s)
'100 BROAD ROAD APT. 3'

>>> re.sub(r'\bROAD\b', 'RD.', s)
'100 BROAD RD. APT. 3'


roman literals :

    why 4 can't be written as IIII

    because at the max 3 times only you can repeat chars I X C M
                                                        1  10 100 1000

L 50
D 500

Note the $ at the end of the pattern....otherwise we can't limit the no of Ms.

>>> pat = '^M{0,2}$'
>>>
>>> re.search(pat, 'MMMM')
>>> re.search(pat, 'MMM')
>>> re.search(pat, 'MM')
<_sre.SRE_Match object at 0x009E8B48>
>>>
>>> re.search(pat, '')
<_sre.SRE_Match object at 0x00A7B6E8>
>>>
>>> re.search(pat, 'Mx')
>>>
>>> re.search(pat, 'MMxM')   

>>>

if pat = '^M{0,2}' ( note with out $,  
re.search(pat, 'MMxM')  will result
>>> re.search(pat, 'MMxM')
<_sre.SRE_Match object at 0x009E8B48>

```

