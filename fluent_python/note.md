# Fluent Python
## Chapter 1

1. 生成器表达式更适合用来创建序列类型，因为其背后遵守了迭代器协议，可以逐个产生元素，更加节省内存

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

2. 元组拆包: unpack a tuple as multiple args

```python
t = (20,8)
divmod(*t) # same to `divmod(20,8)`
```

3. 平行赋值

```python
a, b, *rest = range(5)
# [0, 1, [2, 3, 4, ]]
```

### 序列增量赋值(+= \*=)

#### +=
```python
a+=b
```

+= 可能导致两种结果：
1. a实现了__iadd__方法; 调用此方法。对于可变序列来说a会就地改动，在a本身的内存上增加一个b，相当于`append(b)` 
2. a未实现__iadd__方法; 调用__add__方法。a会和b相加，得到新的对象，赋值给a。

#### \*=
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


### bisect 管理有序序列


