## Chapter3 Text versus Bytes

### 3.1 Characters

#### 3.1.1 Unicode

1. Indentified by `code point` , ranging between 0 and 1114111.

2. Start with `U+` and following 4-6 hexadecimal digits, Ex:'G'=>U+1D11E 


3. Actual Bytes depends on the *encoding* method, such as `utf-8` .

```python
s = 'lang£'
print(len(s)) # 5

b = s.encode('utf8')
print(b)      # b'lang\xc2\xa3'
print(len(b)) # 6 

print(b.decode('utf8'))  # lang£
```

### 3.2 Bytes

changable `bytes` unchangable `bytearray` 

1. Elements in range(0, 255)

2. slice of bytes is also bytes, although contains only one element; But the single element is a int in (0, 255)

```python
s = 'lang£'
b = bytes(s, encoding='utf_8')
print(b[0])  # 108
print(b[:1]) # b'l'
```

***Create Bytes*** :

```python
s = 'lang£'
b = bytes(s, encoding='utf_8')
print(b)  # b'lang\xc2\xa3'
b = bytes.fromhex('31 4B CE A9')
print(b)  # b'1K\xce\xa9'
li = [1, 3, 9, 90, 128, 255]
b = bytes(li)
print(b)  # b'\x01\x03\tZ\x80\xff'
b = bytes(array('h', [-2, -1, 0, 1, 2]))
print(b)  # b'\xfe\xff\xff\xff\x00\x00\x01\x00\x02\x00'
```
