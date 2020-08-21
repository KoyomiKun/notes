## Chapter4 Functions

### 4.1 Function is Object

```python
def factorial(n):
    """return n!

    :n: factor
    :returns: n!

    """
    return 1 if n < 2 else n * factorial(n-1)


print(factorial.__doc__)
return n!

    :n: factor
    :returns: n!
```

### 4.2 Higher-order Function 

the function that return a function OR accept a function as parameter is called `higher-order function` 

#### 4.2.1 Map, Filter and Reduce

`list(map(fact, range(6)))` == `[fact(n) for n in range(6)]` 

`list(map(factorial, filter(lambda n:n % 2, range(6))))` == `[factorial(n) for n in range(6) if n % 2]` 

reduce are always used to sums:

```python
from functools import reduce
from operator import add

print(reduce(add, range(100))) # sum(range(100))
```

### 4.3 Lambda Expression

1. We ***Can't*** assign value in lambda or use `while` , `try` , etc.

2. In common, lambda expression used to be parameters in functions.

### 4.4 Callable Objects

There are 7 types of callable objects in python:

1. `def` or `lambda` created by pythoner 

2. built-in functions implemented with C(`len` , `time.strftime` , etc.)

3. built-in method such as `dict.get` 

4. method defined in class.

5. class.

6. objects implementing `__call__` 

7. generator fuction created by `yield` 

```python
print([callable(obj) for obj in (abs, str, 13)])
# [True, True, False]
```

### 4.5 User-defined Callable Type

*ANY* python objects can be callable iff implement `__call__` 

```python
import random


class BingoCase(object):
    """random list"""

    def __init__(self, items):
        self._items = list(items)
        random.shuffle(self._items)

    def pick(self):
        try:
            return self._items.pop()
        except IndexError:
            raise LookupError('pick from empty BingoCase')

    def __call__(self):
        return self.pick()


bingo = BingoCase(range(3))

print(bingo.pick()) # 0
print(bingo()) # 2
```

The difference of method between class and function:

```python
class C: pass
obj = C()

def func(): pass
print(sorted(set(dir(func)) - set(dir(C))))
#  ['__annotations__', '__call__', '__closure__', '__code__', '__defaults__',
#  '__get__', '__globals__', '__kwdefaults__', '__name__','__qualname__']
```
![attritubes](/home/komikun/Pictures/screenshoots/2020-08-03_11-04-12_screenshot.png)

 
### 4.6 Keyword-only Argument
 
***This function are used to create HTML tags*** 

```python
def tags(name, *content, cls=None, **attrs):
    if cls:
        attrs['class'] = cls
    if attrs:
        attr_str = ''.join(' {}={}'.format(attr, value)
                           for attr, value in attrs.items())
    else:
        attr_str = ''
    if content:
        return '\n'.join('<{} {}>{}</{}>'.format(name, attr_str, c, name) for c
                         in content)
    else:
        return '<{} {} />'.format(name, attr_str)


print(tags('img', 'i dont know', align='right',  cls='/plain/img'))
# <img  align=right class=/plain/img>i dont know</img>
```

### 4.7 Get Parameters' Information

Function objects have an tuple attribute called `__defaults__` to save default value of `positional parameters` and `keyword parameters` , default value of `keyword` is in `__kwdefaults__` , and the name of parameters  is in `__code__` :

```python
def test(name, ass='name', *awg, **kwag):
    pass

print(test.__defaults__)# ('name',)
print(test.__kwdefaults__) # None
print(test.__code__.co_argcount)# 2
print(test.__code__.co_varnames) # ('name', 'ass', 'awg', 'kwag')
```

***Tips*** :

The order of parameters: 1. positional only 2. keyword or positional 3. variable positional 4. keyword only 5. variable keyword 

python *DOES NOT* support 1, but some function implement with C support this(ex:`divmod`).

EX:`def test(a, b=3 , *arg, d, e=5, f, **kargs)` 
> a, b: 2  
\*arg: 3  
d, e, f: 4  
\*\*kargs: 5 

#### 4.7.1 Inspect Module 

`inspect.signature` returns a `inspect.Signature` object with an attribute `parameters` . That is an ordered mapper, mapping name of paras with `inspect.Parameter` .

```python
import inspect
def test(name, ass='name', *awg, **kwag):
    pass
sig = inspect.signature(test)
print(sig) # (name, ass='name', *awg, **kwag)
print(sig.parameters)
# OrderedDict([('name', <Parameter "name">), ('ass', <Parameter "ass='name'">), ('awg', <Parameter "*awg">), ('kwag', <Parameter "**kwag">)])
```
attr `Parameter` alse has its own attrs:`name` , `default` , and `kind` .



### 4.8 Functional Programming

#### 4.8.1 Operator Module

`operator` module provide several functions to santisfy arithmetic operations.

```python
reduce(operator.mul, range(1, n+1))
# reduce(lambda a, b: a*b, range(1, n+1))
sorted(data, key=itemgetter(1))
# sorted(data, key=lambda fields: fields[1])
# itemgetter uses [], so it support any class implement __getitem__
# correspond with itemgetter, there exists attrgetter.
```

#### 4.8.2 Functools Module

As we all know, `functools` module provide `reduce` function to handle elements one by one. In addition, `partial` and `partialmethod` also offer us useful funcs.

***Ex1*** :
```python
from operator import mul
from functools import partial

triple = partial(mul, 3)
print(triple(7)) # 21

print(list(map(triple, range(1, 10)))) # [3, 6, 9, 12, 15, 18, 21, 24, 27]
```

`partial`'s first arg is a callable object, following several binding positional args and keyword args. 

***Ex2*** :
```python
import unicodedata
import functools

nfc = functools.partial(unicodedata.normalize, 'NFC')
s1 = 'cafe\u0301'
s2 = 'café'

print(s1 == s2) # False
print(nfc(s1) == nfc(s2)) # True
```
