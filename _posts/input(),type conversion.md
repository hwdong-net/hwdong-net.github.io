### input

input() function is used to get input from user (keyboard). input() allow user to input information to our program.

We can use a variable refer to the input message which is in fact a string.


```python
your_name = input()
```

    john
    

The friendly program should give the user some hints, prompting the user to enter certain information we want, such as name, age,etc


```python
your_name = input("please input your name:")
your_age = input("please input your age:")
print("Hi,"+your_name+",your age is:"+your_age)
```

    please input your name:john
    please input your age:23
    Hi,john,your age is:23
    


```python
a= float(input("enter first number:"))
b= float(input("enter first number:"))
c= a+b
print("the sum of "+str(a)+" and "+str(b)+" is:"+str(c))
```

    enter first number:2.3
    enter first number:5.6
    the sum of 2.3 and 5.6 is:7.8999999999999995
    

### Explicit type conversion

We want to write a program to get the radius of a circle  from the user and output the area of the circle.


```python
r = input("pleanse enter the radius:")
pi = 3.1415916
area = 2*pi*r*r
print("the area of cieclr with radius "+r+"is:",area);
```

    pleanse enter the radius:2.5
    


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-12-698e6bb5637b> in <module>
          1 r = input("pleanse enter the radius:")
          2 pi = 3.1415916
    ----> 3 area = 2*pi*r*r
          4 print("the area of cieclr with radius "+r+"is:",area);
    

    TypeError: can't multiply sequence by non-int of type 'float'


r is a string(character sequence),but pi is float type. they can do operator `*`

We should convert r from string type to float type.


```python
r = input("pleanse enter the radius:")
r = float(r)
pi = 3.1415916
area = 2*pi*r*r
print("the area of cieclr with radius "+str(r)+"is:",area);
```

    pleanse enter the radius:2.5
    the area of cieclr with radius 2.5is: 39.269895
    


```python
We use `float(r)` to convert r form str to float type, Later `str(r)` to to convert r form float to  str type 
```

We use **type**`(variable)` to convert variable from its original type to type **type**.
This is called **explicit type conversion**

To convert a binary string from a str type to an int type,we should pass 2 to the second parameter:


```python
a = int("1011",2) # parameter represent it is binary str
print(a)
print(type(a))
```

    11
    <class 'int'>
    

To convert a hex string from a str type to an int type:


```python
int('0x2d',16)
```




    45



convert a int to str.


```python
str(45)
```




    '45'




```python
bin(45)
```




    '0b101101'




```python
hex(45)
```




    '0x2d'




```python
oct(45)
```




    '0o55'




```python
convert a str to tuple:
```


```python
tuple("hwdong")
```




    ('h', 'w', 'd', 'o', 'n', 'g')




```python
list("hwdong")
```




    ['h', 'w', 'd', 'o', 'n', 'g']




```python
t = (['a',1],['b',2],['c',3])
dict(t)
```




    {'a': 1, 'b': 2, 'c': 3}




```python
list(t)
```




    [['a', 1], ['b', 2], ['c', 3]]



### Implicit type conversion

Sometimes Python can automatically converts one data type to another data type. This process doesn't need any user involvement. 

When two data with different type involved in an operation.Implicit type conversion try to convert the lower data type (integer) to the higher data type (float) to avoid data loss. But this implicit type conversion may not be successful, for example, converting an int to a str.




```python
a= 2
b =3.14
c = a+b
print(type(a+b))
print(type(c))
```

    <class 'float'>
    <class 'float'>
    


```python
When two int values are divided, they are also automatically converted to float and divided again.
```


```python
print(2/3)
type(2/3)
```

    0.6666666666666666
    




    float




```python
If you want 2 int to do integer division, you can use the operator //
```


```python
print(2//3)
type(2//3)
```

    0
    




    int




```python

```
