## Chapter2 Dict and Set

### 2.1 Mapping

#### 2.1.1 Hashable

1. implement `__hash__()` method and `__qe__()` method

2. atom types `str, bytes, num` are hashable, as well as frozenset. As for `tuple`, iff elements in tuple are all hashable, this tuple is hashable.  
```python
tt = (1, 2, (3, 4, ))
print(hash(tt))  # 3794340727080330424

tt = (1, 2, [3, 4, ])
print(hash(tt))  # TypeError: unhashable type: 'list'
```

***ALL UNCHANGEABLE TYPES ARE HASHABLE***   
except something containing changeable elements

### 2.2 Dict Comprehension

To create a dict:

```python
a = dict(one=1, two=2, three=3)
b = {'one':1, 'two':2, 'three':3}
c = dict(zip(['one','two','three'], [1, 2, 3, ]))
d = dict([('two', 2), ('one', 1), ('three', 3)])
e = dict({'three':3, 'one':1, 'two':2})

# dict comprehension
lists = [('one', 1), ('two', 2), ('three', 3)]
f = {en: num for en, num in lists} # {'one': 1, 'two': 2, 'three': 3}
```

### 2.3 Useful Mapping Functions

1. `d.popitem` : pop the first insert element, with arg `last=True` , pop the last insert element.

2. `d.update(m, [**kargs])` : if m has `keys` method, treat m as mapper; else as iterator (key, value)

3. `d.get(k, default)` : replace `d[k]` , set a default value if can't find k.
 
4. `d.setdefault(k, []).append(new_value)` : same as `if key not in d: d[key] = [] ; d[key].append(new_value)` 

### 2.4 Mappings with Flexible Key Lookup

#### 2.4.1 Defaultdict

`d = defaultdict(list)` return an empty list when `__getitem__` is invoked, including `d[k]` , but excluding `d.get(k)` , instead it returns `None` 

| list | int | float | MyClass                                     | 
|:-----|:----|:------|:--------------------------------------------|
| []   | 0   | 0.0   | <__main__.MyClass object at 0x7f12b7f25430> | 

#### 2.4.2 __missing\_\_

If a class extends `dict` and provide `__missing__` method, when key is not found, this method will be called.
y

`__missing__` example: 

```python
def __missing__(self, key):
	if isinstance(key, str):
		raise KeyError(key)
	return self[str(key)]
```

***Tips***: `d.keys()` returns a view, which is responsed quickly on query.

### 2.5 Other Dicts

#### 2.5.1 OrderedDict

Keep the dict in insertion time order and pop the last element.

#### 2.5.2 ChainMap

A chain of mappers, look up all mappers inside it.

for example, in python official varible lookup progress:

```python
import builtins
pylookup = ChainMap(locals(), globals(), vars(builtins))
```

#### 2.5.3 Counter

count the elements in hashable objects 

```python
from collections import Counter
ct = Counter('abcdabcdabca')
print(ct)               # Counter({'a': 4, 'b': 3, 'c': 3, 'd': 2})
ct.most_common(2)       # [('a', 4), ('b', 3)]
ct + Counter('ddaactg') # Counter({'a': 6, 'b': 3, 'c': 4, 'd': 4, 't': 1, 'g': 1})
```

### 2.6 UserDict

In order to create a new dict class, it is a better choice to extends `UserDict` rather than simple `dict` .

Indeed, `data` attribute of `UserDict` is subclass of `dict` .

For examle:
```python
from collections import UserDict


class StrKeydict(UserDict):

    """User Dict with string key"""

    def __init__(self):
        UserDict.__init__(self)

    def __missing__(self, key):
        if isinstance(key, str):
            raise KeyError(key)
        return self[str(key)]

    def __contains__(self, key):
        return str(key) in self.data

    def __setitem__(self, key, item):
        self.data[str(key)] = item

```

### 2.7 Unchangable Mapper

`MappingProxyType` is a dynamic view of mapper:

```python
# Create a MappingProxyType
d = {1:'A'}

d_proxy = MappingProxyType(d)
d_proxy[2] = 'X' # TypeError: 'mappingproxy' object does not support item assignment

d[2] = 'R'
d_proxy # {1: 'A', 2: 'R'} 
```
### 2.8 Set

elements in set must be *hashable* , because to ensure uniqueness , python use `__hash__` and `__eq__` to delete duplicated elements.

***Tips*** : `Set` is not hashable, but `frozenset` is.

#### 2.8.1 Create a set

```python
# Empty set
s = set() # s = {} create a dictory
# Set literals
s = {1} # faster than s = set([1])
# set comprehension
s = {chr(i) for i in range(32, 256) if 'SIGN' in name(chr(i), '')} # {'=', '$', '£', '¥', '<', '°', '®', '§', '÷', 'µ', '¤', '×', '>', '#', '+', '¢', '©', '¶', '¬', '±', '%'}
```

#### 2.8.2 Set Operations

```python
s1 = {1, 2, 4, 6, 7, }
s2 = {4, 5, 7, 3, 6, }

print(s1 & s2) # {4, 6, 7}
print(s1 | s2) # {1, 2, 3, 4, 5, 6, 7}
print(s1 - s2) # {1, 2}
```

####  2.8.3 Hash Table

Hash table is a sparse array, whose cells called `Bucket`. A `bucket` contains 2 fields: reference to keys and reference to values.
 
In order to keep 1/3 spaces empty, python will *copy original array* into a new location having bigger space.

***Hash*** :

When we tend to insert an element into a hash table, the first step is to calculate the *hash value* with `hash()` method.

> `==` means 'hash equal', `hash(1.0) == hash(1)` is true, but it doesn't means `1` and `1.0` have the the data structure.

> After python 3.3, the calculation of hash values of `str`, `bytes`, and `datetime` add salting progress. `Salt` is a constant value formed randomly at the start of python thread.

***Progress of retrieving item***:
![progress of retrieving item](/home/komikun/Pictures/screenshoots/2020-08-01_19-59-11_screenshot.png) 


***Tips*** :

1. According to `CPython` , if an int object's value can be arraged in a machine `word` , its hash value equals to its value

2. When facing *Hash collision* , there is an special alogrithm to handle it

Keys: `Quadratic Probing` , `perturb` 

[Collision Handling Method](https://www.kawabangga.com/posts/2493) 

Keys: `Why use perturb` , `Hash collision attack` 

[Hash Collision Attack](https://blog.zhenlanghuo.top/2017/05/17/%E5%93%88%E5%B8%8C%E7%A2%B0%E6%92%9E%E6%94%BB%E5%87%BB/) 
```python
def lookup(d, key):
    perturb = j = hash(key)
    while True:
        cell = d.data[j % d.size]
        if cell.key is EMPTY:
            raise IndexError
        if cell.key is not DELETED and (cell.key is key or cell.key == key):
            return cell.value
        j = (5 * j) + 1 + perturb
        perturb >>= PERTURB
```
