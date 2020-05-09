## 1. object and Variable

every thing is an object, variable is a name of an object.

### 1.1 object

each object has: **type**, **value(content)**, **id**.

you can use type() functiong to find the type of an object. 


```python
type(3)
```




    int




```python
type(3.14)
```




    float




```python
type("hwdong.net")
```




    str




```python
type(False)
```




    bool




```python
the data type of these objects are different.
```

Each object has a unique ID, it is the address of that object in memory。

you can use id() to find the id of an object.


```python
id(3)
```




    140721594999248




```python
id(3.14)
```




    2830338663824




```python
id(False)
```




    140721594476912




```python
id("hwdong.net")
```




    2830341107888




```python
you can use hex() convert id to hex format:
```


```python
hex(id(3.14))
```




    '0x292fd780030'



### 1.2 variable

You can give an object a name,this name is called **variable**. Then the variable refer to the object.


```python
a = 2
pi = 3.14
my_homepage = "hwdong.net"
print(a)
print(pi)
print(my_homepage)
```

    2
    3.14
    hwdong.net
    

The name of varialbe can changed to other object anytime.


```python
print(id(a))
print(id(pi))
a =  pi
print(id(a))  # a and pi all refer to the object 3.14
```

    140721594999216
    2830340981840
    2830340981840
    

### 1.3 date type

Data types represent a kind of value which determines what operations can be performed on that data.

Numeric, non-numeric and Boolean (true/false) data are the most used data types. 

**Numeric** 
- int: represent a positive or negative integet number.
- float: represent a real number,aften called floating point real number
- complex: represent a complex number,such as: 2+3j


```python
type(2+3j)
```




    complex



**Boolean**:
- bool: onlye two value: True,False

### Sequence Type

A sequence is an ordered collection of similar or different data types. Python has the following built-in sequence data types:

- str: represetn a string. A string value is a collection of one or more characters put in single, double or triple quotes.
- list: a list object is an ordered collection of one or more data items, not necessarily of the same type, put in square brackets.
- tuple: a tuple object is an ordered collection of one or more data items, not necessarily of the same type, put in parentheses.


```python
type([1,2,3,4,5])
```




    list




```python
type((1,2,3,4,5))
```




    tuple



you can use index to access the element of a sequence object. Index start from 0 to n-1 (where n is length of the object,
the number of elements) 


```python
s = "hwdong.net"
l = [1,2,3,4,5]
t = (1,2,3,4,5)

print(s[1])
print(l[1])
print(t[1])
```

    w
    2
    2
    

you can use len() to find the length of a sequence object:


```python
print(len(s),len(l),len(t))
```

    10 5 5
    

you can use negtive integer as index. the negtive index should be from -1 to -n.


```python
print(s[-1])
print(l[-1])
print(t[-1])
```

    t
    5
    5
    

### Dictionary
- dict: A dictionary object is an unordered collection of data in a key:value pair form. A collection of such pairs is enclosed in curly brackets. 

you can sue key to find the value cooresponding to the key:


```python
d = {"john":32223426,"wang":453451098}

d["john"]
```




    32223426



### String

String literals are written by enclosing a sequence of characters in single quotes ('hwdong'), double quotes ("hwdong") or triple quotes ('''hwdong''' or """hwdong""").

To represent the single quote in a string enclosed by quotes, You can use **escape sequences** to represent a special character. An escape sequences start with a escape character: backslash `\`。

For example, `\"` represent the character `"`,`\\`represent the character `\`. Following are other escape sequences

- \a 	Bell or alert.
      result::Bell sound
- \b	Backspace, 	
      "ab\bc" result::	ac
- \f	Formfeed	
         "hello\fworld"	result::hello world
- \n	Newline	
          "hello\nworld"	result:Hello
- \nnn	Octal notation, where n is in the range 0-7. 
        '\101' c:	A
- \t	Tab	 
        'Hello\tPython'	result: Hello Python
- \xnn	Hexadecimal notation, where n is in the range 0-9, a-f, or A-F. 	
       '\x41'	result:A




```python
print("hello\"nworld")
print("ab\bc")
print("hello\tworld")
print("hello\nworld")
print("hello\fworld")
print("\x41")
```

    hello"nworld
    abc
    hello	world
    hello
    world
    helloworld
    A
    

Python raw string is created by prefixing a string literal with ‘r’ or ‘R’. Python raw string treats backslash (\) as a literal character. This is useful when we want to have a string that contains backslash and don’t want it to be treated as an escape character.


```python
print(r"hello\nworld")
```

    hello\nworld
    

[https://www.tutorialsteacher.com/python/python-data-types](https://www.tutorialsteacher.com/python/python-data-types)


```python

```
