# Fluent Python
## Chapter1 List

### 1.1 生成器表达式
生成器表达式更适合用来创建序列类型，因为其背后遵守了迭代器协议，可以逐个产生元素，更加节省内存

```python
# 生成tuple
symbols = '(*^%$#'
t = tuple(ord(symbol) for symbol in symbols)
# 计算笛卡尔积
colors = ['black', 'white']

sizes = ['S', 'M', 'L']

for tshirt in ("{} {}".format(c, s) for c in colors for s in sizes):
    print(tshirt)
```

### 1.2 元组拆包
unpack a tuple as multiple args

```python
t = (20,8)
divmod(*t) # same to `divmod(20,8)`
```

平行赋值

```python
a, b, *rest = range(5)
# [0, 1, [2, 3, 4, ]]
```

### 1.3 序列增量赋值(+= \*=)

#### 1.3.1 +=
```python
a+=b
```

+= 可能导致两种结果：
1. a实现了__iadd__方法; 调用此方法。对于可变序列来说a会就地改动，在a本身的内存上增加一个b，相当于`append(b)` 
2. a未实现__iadd__方法; 调用__add__方法。a会和b相加，得到新的对象，赋值给a。

#### 1.3.2 \*=
和`+=` 一样，\*=也是两种结果，区别在于调用的是__imul__和__mul__方法。

```python
l = [1, 2, 3, ]
id(l) # 140533065687936
l *= 2
id(l) # 140533065687936 一样，因为可变对象有__imul__方法

--------

l = (1, 2, 3, )
id(l) # 140533065380992
l *= 2
id(l) # 140533065342016 不一样，因为不可变对象没有__imul__方法

```

***一个有趣的问题*** :
```python
t = (1, 2, [30, 40, ])
t[2] += [50, 60, ]
```

上述代码的执行结果是什么：

A. t变成(1, 2, [30, 40, 50, 60, ])

B. tuple 不支持非元素赋值，抛出`TypeError` 异常

C. A B 都不对

D. A B 都对

`Answer: D` 

[python tutor](http://pythontutor.com/visualize.html#mode=edit) 的结果显示，不仅t的值改变了， 同时还抛出错误

> 原因是***增量赋值不是一个原子操作*** 
> 
> 先完成内部列表的相加操作，再将加好的内容赋值给元组
> 
> 第一步能够成功，第二步却会失败

所以***最好不要把可变对象放在元组里面***


### 1.4 bisect 管理有序序列

#### 1.4.1 bisect.bisect

`bisect(haystack, needle)` : find a needle in a haystack

position must satisfy that if needle insert into haystack[index], haystack keeps in ascending order 

Ex: `bisect([1, 3, 4, 6, ], 2)` returns `1` 

***Options:*** 

1. `bisect(haystack, needle, lo=1, hi=2 )`  
means search in `range(lo, hi)` 
2. `bisect_left(haystack, needle)`  
compared to `bisect` and `bisect_right` , `bisect_left` returns the index that `haystack[:index+1]` < needle rather than `haystack[:index+1]` <= needle 


#### 1.4.2 bisect.insort

`insort(haystack, needle)` : find the place and insert into it

*Equals to* 
```python
index = bisect(haystack, needle)
haystack.insert(needle, index)
```
but faster than two steps
#### Usages:
Bisect module suits for GRADING:

```python
def grading(score, break_points=[60, 80, 90, ], grades='FCBA'):
    index = bisect.bisect(break_points, score)
    return grades[index]
print(grading(100)) # A
print(grading(60)) # C
print(grading(40)) # F
```

### 1.5 Array

If we need *a list only consists of numbers*, `array.array`  is more efficient than `list`

Advantages:
1. cost less time and less memory 

2. `array.tofile` and `array.fromfile` 

***Tips*** :From python 3.4, `array.sort()` are not supported. For sorting an array, we should say `a = array(typecode, sorted(a))` 

#### 1.5.1 MemoryView

As the name implies, MemoryView module provides views of memory, which means one piece of memory can be considered as float, int, list, char, etc. 

```python
from array import array

nums = array('h', [-2, -1, 0, 1, 2, ])

memv = memoryview(nums)
print((len(memv), memv[0]))  # (5, -2)

memv_oct = memv.cast('B')
print(memv_oct.tolist())  # [254, 255, 255, 255, 0, 0, 1, 0, 2, 0]
```

'h' means `short` , 'B' means `unsigned short` 

