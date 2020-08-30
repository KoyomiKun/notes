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
***Compare between == and is*** :

1. `==` compare the value , and `is` compare the tag

2. `is` is faster than `==` 









