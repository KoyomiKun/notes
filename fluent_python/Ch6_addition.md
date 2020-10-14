### 6.5 Defend the Changable Parametersm

In [1]: <!--{"msg_id":1,"type":"code"}-->
```python
class TwilightBus:
    
    def __init__(self, passengers=None):
        if passengers is None:
            self.passengers = []
        else:
            self.passengers = passengers
			# self.passengers = list(passengers)

    def pick(self, name):
        self.passengers.append(name)

    def drop(self, name):
        self.passengers.remove(name)

```
Out [1]:
```
223
```

pay attention to `self.passenger=passengers` . `self.passengers` is an alias of `passengers` .

### 6.6 Del and garbage collection 

`del` only delete the reference, not the object. IFF this ref is the last one or not accessible, the object may be recycled.

***For CPython implementation*** :

1. Reference counting

Ways to increase the reference count:

- Assigning an object to a variable

- Adding an object to a structure(list, property, ...)

- Passing an object as an argument to a function

In [4]: <!--{"msg_id":32,"type":"code"}-->
```python
import sys

a = 'my-string' 
sys.getrefcount(a)
```
Out [4]:
```
3
```
***Tip:*** In `PYTHON REPL` , refcount = 2, so `ipython` may add an additional ref to variable `a` .


In [5]: <!--{"msg_id":33,"type":"code"}-->
```python
b = [a]
sys.getrefcount(a)
```
Out [5]:
```
3
```

In [6]: <!--{"msg_id":34,"type":"code"}-->
```python
c = {'key': a}
sys.getrefcount(a)
```
Out [6]:
```
4
```
In [7]: <!--{"msg_id":35,"type":"code"}-->
```python
del b
sys.getrefcount(a)
```
Out [7]:
```
3
```

In [8]: <!--{"msg_id":36,"type":"code"}-->
```python
del c
sys.getrefcount(a)
```
Out [8]:
```
2
```

<++> 



2. Generational garbage collection


