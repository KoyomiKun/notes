## Chapter6 Object-orient

For quoted objects, names are regarded as sticker.

In [1]: <!--{"msg_id":1,"type":"code"}-->
```python
a = [1, 2, 3]
b = a
a.append(4)
b
```
```
True
```
```
123
```
```
[['abc', 'bbb', 'scs'], 'dbc']
[['abc', 'bbb', 'scs'], 'dbc']
```
```
0
1
2
```
Out [1]:
```
[1, 2, 3, 4]
```

### 6.1 Symbols, Equalution and Alias

`lewis` is alias name of `charles` 

In [5]: <!--{"msg_id":5,"type":"code"}-->
```python
charles = {'name':'C. L. D.', 'born':1832}
lewis = charles
print(lewis is charles)
id(charles), id(lewis)
```
```
True
```
```
NameError
---------------------------------------------------------------
NameError                     Traceback (most recent call last)
<ipython-input-5-35fbbc49b71c> in <module>
      5 x = 1
      6 x = 2
----> 7 f(x, y)

NameError: name 'y' is not defined
```
Out [5]:
```
(140020350533824, 140020350533824)
```


another one with the same name and birthyear

In [6]: <!--{"msg_id":6,"type":"code"}-->
```python
fake = {'name':'C. L. D.', 'born':1832}
fake is charles
```
```
NameError
---------------------------------------------------------------
NameError                     Traceback (most recent call last)
<ipython-input-6-35fbbc49b71c> in <module>
      5 x = 1
      6 x = 2
----> 7 f(x, y)

NameError: name 'y' is not defined
```
Out [6]:
```
False
```

In [2]: <!--{"msg_id":2,"type":"code"}-->
```python
for i in range(3):
	print(i)
```
```
0
1
2
```
```
123
```
```
[['abc', 'bbb', 'scs'], 'dbc']
[['abc', 'bbb', 'scs'], 'dbc']
```
***Compare between == and is*** :

1. `==` compare the value , and `is` compare the tag

2. `is` is faster than `==` 


### 6.2 Copy and DeepCopy 

Use function `copy` and `deepcopy` to choose whether or not copy the cotent of inner pointers.

In [3]: <!--{"msg_id":3,"type":"code"}-->
```python
import copy

innner_arr = ['abc', 'bbb', 'scs']
arr = [innner_arr, 'dbc']

c1 = copy.copy(arr)
c2 = copy.deepcopy(arr)
print(c1)
print(c2)

```
```
[['abc', 'bbb', 'scs'], 'dbc']
[['abc', 'bbb', 'scs'], 'dbc']
```
```
123
```

In [4]: <!--{"msg_id":4,"type":"code"}-->
```python
innner_arr.remove('abc')
print(c1)
print(c2)
```
```
[['bbb', 'scs'], 'dbc']
[['abc', 'bbb', 'scs'], 'dbc']
```

***IF there exists circular references, problem will be complex. As for function deepcopy, it remembers the objects it has copyed.*** 

By overwrite `__copy__()` and `__deepcopy__()` , we can define our own copy function


### 6.3 Parameters of Function 

The only way to send parameters is 'call by sharing'.

But actually, `tuple` , `int` and other unchangable variables will ***not*** be changed by function.

In [8]: <!--{"msg_id":8,"type":"code"}-->
```python
def f(a, b):
	a+=b
	return a

x = 1
y = 2
f(x, y)
```
Out [8]:
```
3
```

As for (x, y)

In [9]: <!--{"msg_id":9,"type":"code"}-->
```python
x, y
```
Out [9]:
```
(1, 2)
```

### 6.4 DON'T USE DEFAULT CHANGABLE PARAS!

For Example:

In [10]: <!--{"msg_id":10,"type":"code"}-->
```python
class HauntedBus:
	
	def __init__(self, passengers=[]):
		self.passengers = passengers
	
	def pick(self, name):
		self.passengers.append(name)

	def drop(self, name):
		self.passengers.remove(name)
```


Now If we has two bus with default para:

In [11]: <!--{"msg_id":11,"type":"code"}-->
```python
bus1 = HauntedBus()
bus1.pick('Howdy')
print(bus1.passengers)


bus2 = HauntedBus()
print(bus2.passengers)

```
```
['Howdy']
['Howdy']
```

Unbelieable! The passengers in `bus1` show up in `bus2`!

***The default value was calculated when loading modules, so the DEFAULT VALUE was recorded as an attribute of this function*** 

In [12]: <!--{"msg_id":12,"type":"code"}-->
```python
print(dir(HauntedBus.__init__))
print(HauntedBus.__init__.__defaults__)
```
```
['__annotations__', '__call__', '__class__', '__closure__', '__code__', '__defaults__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__get__', '__getattribute__', '__globals__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__kwdefaults__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
(['Howdy'],)
```


