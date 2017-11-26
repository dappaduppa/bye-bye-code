---
layout: post
title:  "numpy!"
date:   2017-08-03 11:52:46 +0900
categories: Python, NumPy, TensorFlow
---

**NumPy basic functions**  

*shape* => (3, 5) i.e. array shape  
*ndim* => 2  i.e. 2 dim or n dim  
*dtype* => data type = int64  
*dtype.name* => int64  
*itemsize* => 8 ( 64 / 8 ) byte   
*size* => 15 ( no. of elements)  
*type(y)* => ndarray ( n dimensional array ?!)  
*len(y)* => no. of rows in array ( where as size specifies total no. of elements)  
  for 1 dim. array len == size 
**frequent mistake:** 


```python
>>> a = np.array(1,2,3,4)  
Traceback (most recent call last):  
  File "<stdin>", line 1, in <module>  
ValueError: only 2 non-keyword arguments accepted  
>>> a = np.array([1,2,3,4])  
>>>  
>>> a  
array([1, 2, 3, 4])  
>>>  
```

two ways ( ) or []

```python
>>> a = np.array( [[ 1.5, 2, 3], [4, 5, 6]] )
>>> a
array([[ 1.5,  2. ,  3. ],
       [ 4. ,  5. ,  6. ]])
>>>
>>> a = np.array([(1.5, 2, 3), (4, 5, 6)])
>>> a
array([[ 1.5,  2. ,  3. ],
       [ 4. ,  5. ,  6. ]])
>>>
```

TODO:  
histograms

Install matplotlib on mac  

[so link](https://stackoverflow.com/questions/7157330/problem-importing-matplotlib-mlab-and-pyplot-in-python-2-7-on-mac-osx-10-6)

[blog link](http://telliott99.blogspot.jp/2011/07/matplotlib-on-os-x-lion-revised.html)

Vector stacking:
-----

  **vstack**  
  **hstack**  
  **dstack**
  **column_stack**

```python
a1 = np.array([1,2,3,4,5])
b1 = np.array([6,7,8,9,10])

ab = np.vstack([a1, b1])  


#arange  (NOTE: a range, NOT arrange) 
x1 = np.arange(5)
y1 = np.arange(0,10,2)
z1 = np.hstack([x1, y1])
z2 = np.vstack([x1, y1])

```

Automatic reshaping with **shape**  

d.shape = 3, -1, 5 # -1 whatever is needed  



---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
