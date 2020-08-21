## Chapter5 Closure and Decorators

### 5.1 Basic Decorators

Decorator *replace* function with another function:

```python
def deco(func):
    def inner():
        print('running inner()')
    return inner


@deco
def target():
    print('running target()')


target() # running inner()
# equals to :
# def target():
#	print('running target()')
# target = decorate(target) 	

```

***Tips:*** 
Python execute `decorator` right after the difinition, exactly, importing modules.


### 5.2 Closure 

```python
class Averager():
    """Averager Caculator"""

    def __init__(self):
        self.sum = 0
        self.count = 0

    def __call__(self, new_value):
        self.sum += new_value
        self.count += 1
        return self.sum/self.count


avg = Averager()
print(avg(1)) # 1.0
print(avg(2)) # 1.5
print(avg(3)) # 2.0
```

Another version:

```python
def make_averager():
    count = 0
    total = 0

    def averager(new_value):
        nonlocal count, total
        count += 1
        total += new_value
        return total / count
    return averager


avg = make_averager()
print(avg(1))                           # 1.0
print(avg(2))                           # 1.5
print(avg(3))                           # 2.0
print(avg.__code__.co_varnames)         # ('new_value',)
print(avg.__code__.co_freevars)         # ('count', 'total')
print(avg.__closure__[0].cell_contents) # 3
```

![free variable](/home/komikun/Pictures/screenshoots/2020-08-05_21-46-47_screenshot.png) 

`nonlocal` explicit express those variates are free-variates.

***HOW TO USE DECORATORS*** :

```python
import random
import time
from functools import wraps


def clock(func):
    @wraps(func)
    def clocked(*args, **kwargs):
        t0 = time.perf_counter()
        result = func(*args)
        elapsed = time.perf_counter() - t0

        name = func.__name__
        arg_lst = []
        if args:
            arg_lst.append(', '.join(repr(arg) for arg in args))
        if kwargs:
            pairs = ['{}={}'.format(k, w) for k, w in sorted(kwargs.items())]
            arg_lst.append(', '.join(pairs))
        arg_str = ', '.join(arg_lst)

        print('[{:8f}] {}({}) -> {}'.format(elapsed, name, arg_str, result))
        return result
    return clocked


@clock
def test(li: list):
    for i in li:
        pass
    return len(li)


test([random.randint(4, 10) for _ in range(5)])
```

`wraps` copy `__name__` , `__doc__` , etc. from `func` to `clocked` .

### 5.3 Decorators in Standard Lib 

#### 5.3.1 Lru_cache

`functools.lru_cache` realize the *memoization* function by caching the return result.

```python
@clock
@lru_cache()
def test(n: int):
    if n < 2:
        return n
    return test(n-1) + test(n-2)


test(30)
# [0.000720s] test(30) -> 832040
# [71.819455s] test(30) -> 832040
```

`lru_cache` tips:

1. `lru_cache` has two parameters `maxsize` and `typed` , the prior one define the max number of elements it caching, and the later one define whether distinguish 1 and 1.0 (int and float type) or not.

2. `maxsize` had better to be 2^n.

3. `lru_cache` use dict to save the result, parameters as key and return result as value. So all parameters *MUST BE* hashable.

#### 5.3.2 Singledispatch

As the name says, to understand this method , we should divide `singledispatch` into `single` and `dispatch` .

***Dispatch*** :

`Dispatch` means this decorator helps us dispatch different missions to different kinds of parameters.

```python
from functools import singledispatch
from collections import abc
import numbers
import html


@singledispatch
def htmlize(obj):
    content = html.escape(repr(obj))
    return '<pre>{}</pre>'.format(content)


@htmlize.register(str)
def _(text):
    content = html.escape(text).replace('\n', '<br>\n')
    return '<p>{0}</p>'.format(content)


@htmlize.register(numbers.Integral)
def _(n):
    return '<pre>{0}(0x{0:x})</pre>'.format(n)


@htmlize.register(tuple)
@htmlize.register(abc.MutableSequence)
def _(seq):
    inner = '<\li>\n<li>'.join(htmlize(item) for item in seq)
    return '<ul>\n<li>' + inner + '</li>\n</ul>'
```

In common, we use `<<base_function>>.register(<<type>>)` to decorate generic functions.`base function` should be abstract class such as `numbers.Integral` and `abc.MutableSequence` , rather than `int` or `list` .

### 5.4 Multiple Decorators

```python
@d1
@d2
def f():print('f')

# equals to:
def f(): print('f')
f = d1(d2(f))
```



### 5.5 Decorators with Parameters

In [2]: <!--{"msg_id":2,"type":"code"}-->
```python
registry = []

def register(func):
	print('running register{}'.format(func))
	registry.append(func)
	return func

@register
def f1():
	print('running f1()')

print('running main()')
print('registry -> ', registry)
f1()
```
```
running register<function f1 at 0x7ff27678e8b0>
running main()
registry ->  [<function f1 at 0x7ff27678e8b0>]
running f1()
```
Out [2]:
```
True
```

Register Ver.2 : Get a para 'active':

In [3]: <!--{"msg_id":3,"type":"code"}-->
```python
registry = set()
def register(active = True):
	def decorate(func):
		print('running register(active={})->decorate({})'.format(active, func))
		if active:
			registry.add(func)
		else:
			registry.discard(func)
		return func
	return decorate

@register(active=False)
def f1():
	print('running f1')

@register()
def f2():
	print('running f2')

def f3():
	print('running f3')

```
```
running register(active=False)->decorate(function f1 at 0x7ff27678e940)
running register(active=True)->decorate(function f2 at 0x7ff27678eca0)
```
Out [3]:
```
(140020299633856, 140020299633856)
```
```
True
```
```
True
```

In [4]: <!--{"msg_id":4,"type":"code"}-->
```python
registry
```
```
True
```
Out [4]:
```
{function __main__.f2()}
```
