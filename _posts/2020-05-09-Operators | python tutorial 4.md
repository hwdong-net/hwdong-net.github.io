---
layout:       post
title:        "Operators | python tutorial 4"
subtitle:     "Operators | python tutorial 4"
date:         2020-05-09 19:04:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - python
---


## 1. operators

Operators are used to perform operations on data object(variables and values).
Python divides the operators in the following groups:

+ Arithmetic operators
+ Assignment operators
+ Comparison operators
+ Logical operators
+ Identity operators
+ Membership operators
+ Bitwise operators

#### Arithmetic Operators

 + +:	Add two operands or unary plus
 + -: Subtract right operand from the left or unary minus
 + /: Divide left operand by the right one (always results into float)
 + %: Modulus - remainder of the division of left operand by the right
 + //: Floor division - division that results into whole number adjusted to the left in the number line
 + ```**```: Exponent - left operand raised to the power of right




```python
x = 12
y = 5

print("x=",x)
print("y=",y)


print('x + y =',x+y)
print('x - y =',x-y)

print('+x =',+x)
print('-x =',-x)


print('x * y =',x*y)

print('x / y =',x/y)

print('x // y =',x//y)

print('x ** y =',x**y)
```

    x= 12
    y= 5
    x + y = 17
    x - y = 7
    +x = 12
    -x = -12
    x * y = 60
    x / y = 2.4
    x // y = 2
    x ** y = 248832
    

#### Comparison operators

Comparison operators are used to compare values. It returns either True or False according to the condition.

+ `==` :	Equal
+ `!=` :	Not equal
+ `>` : Greater than
+ `<`	: Less than
+ `>=` :	Greater than or equal to
+ `<=` :	Less than or equal to



```python
x = 12
y = 5

print("x=",x)
print("y=",y)

print('x > y is',x>y)

print('x < y is',x<y)

print('x == y is',x==y)

print('x != y is',x!=y)

print('x >= y is',x>=y)

print('x <= y is',x<=y)
```

    x= 12
    y= 5
    x > y is True
    x < y is False
    x == y is False
    x != y is True
    x >= y is True
    x <= y is False
    

### Logical Operators

**not**,**or**,**and**

The **None** keyword is used to define a null value, or no value at all.

+ not: **not x**   means: True if x is False or 0 or None, False if x is True or not 0 or not None


```python
print(not False)
print(not 0)
print(not None)
print(not True)
print(not 3)
print(not not None)
```

    True
    True
    True
    False
    False
    False
    

+ or: **x or y**   means: if x is not None or True,result is x; if x is None or False, otherwise the result is y 


```python
print(False or 3)
print(0 or False)
print(None or "hwdong")
print(True or None)
print('hello' or False)
print("hello" or None)
print('hello' or 3)
```

    3
    False
    hwdong
    True
    hello
    hello
    hello
    

+ and: **x and y**   means: if x is None or 0 or False, the result is x,otherwise the result is y


```python
print(False and 3)
print(0 and False)
print(None and "hwdong")
print(True and None)
print('hello' and False)
print('hello' and None)
print('hello' and 3)
```

    False
    0
    None
    None
    False
    None
    3
    

### Bitwise operators

&, |, ~,^,>>, <<

Every value is represented in binary digits. For example, 2 is 10 in binary and 7 is 111.
We can use the format() method of str to see the binary digits of an operand:


```python
a = 37
print('a = ','{:08b}'.format(a))
```

    a =  00100101
    

 Bitwise operators act on operands as if they were strings of binary digits.
 
**x & y**
Does a "bitwise and". Each bit of the output is 1 if the corresponding bit of x AND of y is 1, otherwise it's 0.

**x | y**
Does a "bitwise or". Each bit of the output is 0 if the corresponding bit of x AND of y is 0, otherwise it's 1.

**x ^ y**
Does a "bitwise exclusive or". Each bit of the output is the same as the corresponding bit in x if that bit in y is 0, and it's the complement of the bit in x if that bit in y is 1.


```python
a = 37
b = 22

print('a = ','{:08b}'.format(a))
print('b = ','{:08b}'.format(b))
print('a&b=','{:08b}'.format(a&b))
print('a|b=','{:08b}'.format(a|b))
print('a^b=','{:08b}'.format(a^b))

```

    a =  00100101
    b =  00010110
    a&b= 00000100
    a|b= 00110111
    a^b= 00110011
    

**~x**

Returns the complement of x - the number you get by switching each 1 for a 0 and each 0 for a 1. This is the same as -x - 1.


```python
print('a =  ','{:08b}'.format(a))
print('~a = ','{:08b}'.format(~a))
```

    a =   00100101
    ~a =  -0100110
    

**x << y**

Returns x with the bits shifted to the left by y places (and new bits on the right-hand-side are zeros). This is the same as multiplying x by 2**y.

**x >> y**

Returns x with the bits shifted to the right by y places. This is the same as //'ing x by 2**y.


```python
print('b =   ','{:08b}'.format(b))
print('b<<2: ','{:08b}'.format(b<<2))    # b的二进制左移2位，右边低位补充0
print('b>>2: ','{:08b}'.format(b>>2))    # b的二进制右移2位，左边高位补充0
print('~b:   ','{:08b}'.format(~b))      # ~b就是-(b+1)，b=22的补是-23
print(b)
print(b<<2) 
print(b>>2)
print(~b)
```

    b =    00010110
    b<<2:  01011000
    b>>2:  00000101
    ~b:    -0010111
    22
    88
    5
    -23
    

### Assignment operators

Assignment operators are used in Python to assign values to variables.

a = 5

Means define a variable refer to object 5. a is not a box, it is just a name. When do:

a = 7

Means define a new variable refer to object 7


```python
a = 5
print(id(a))
a = 7
print(id(a))
```

    140732421153296
    140732421153360
    

There are various compound operators in Python like a += 5 that adds to the variable and later assigns the same. It is equivalent to a = a + 5. But the two a  refer to different object.


```python
a=  3
b = a
print(id(a))
a = a+5
print(id(b))
print(id(a))
a +=5
print(id(a))
```

    140703196305120
    140703196305120
    140703196305280
    140703196305440
    

The three a refer to three objects.

Assignment operator = can be combined with algorithm operators and  Bitwise operators. 

![](./imgs/assignment_operators.png)

### Special operators

### Identity operators
to test if two objects are the same object.

+ **is**:
+ **is not**


```python
a = 5
b = 5
x = 'hwdong'
y = "hwdong"
print(a is b)
print(a is not b)
print(x is y)
print(x is not y)
a = [1,2,3]
b = [1,2,3]
print(a is b)
print(a is not b)
```

    True
    False
    True
    False
    False
    True
    

#### Membership operators

+ **in**
+ **not in**

They are used to test whether a value or variable is found in a sequence (string, list, tuple, set and dictionary).


```python
x = 'hwdong'

print('H' in x)
print('hello' not in x)
print('dong'  in x)

y = {1:'a',2:'b'}
print(1 in y)     # test is key 1  in y
print('a' in y)    # test is key 'a'  in y
```

    False
    True
    True
    True
    False
    

## Operator precedence 

operators have different precedence. Operator precedence affects how an expression is evaluated.


```python
3+5*2
```




    13



[https://www.mathcs.emory.edu/~valerie/courses/fall10/155/resources/op_precedence.html](https://www.mathcs.emory.edu/~valerie/courses/fall10/155/resources/op_precedence.html)

You don't remember Operator precedence. You can use parentheses to change the order for computations.


```python
(3+5)/(2-6)
```




    -2.0



[https://realpython.com/python-operators-expressions/](https://realpython.com/python-operators-expressions/)

[https://www.w3schools.com/python/python_operators.asp](https://www.w3schools.com/python/python_operators.asp)

[https://www.programiz.com/python-programming/operators](https://www.programiz.com/python-programming/operators)


```python

```
