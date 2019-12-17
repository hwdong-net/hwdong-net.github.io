---
layout:       post
title:        "Stanford 231课程：Python Numpy Tutorial教程"
subtitle:     "cs231 course：Python Numpy Tutorial"
date:         2018-05-09 13:51:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - Python 
    
---    
     

# Python Numpy 教程(Python Numpy Tutorial)

这篇[教程](https://cs231n.github.io/python-numpy-tutorial/)来自[Justin Johnson](https://cs.stanford.edu/people/jcjohns/)。


我们将使用Python编程语言来完成本课程的所有作业。Python是一门非常好的通用编程语言，凭借一些流行的库（numpy, scipy, matplotlib），成为了一个强大的科学计算环境。

我们期望你们中大多数人对于Python语言和Numpy库有一些经验，对于其他读者，这篇教程可以帮助你们快速了解Python编程语言和用Python进行科学计算。

对于有Matlab的读者，我们推荐阅读[numpy for Matlab users](http://wiki.scipy.org/NumPy_for_Matlab_Users)。

你还可以查看由[Volodymyr Kuleshov](http://web.stanford.edu/~kuleshov/)和[Isaac Caswell](https://symsys.stanford.edu/viewing/symsysaffiliate/21335)为课程[CS 228](http://cs.stanford.edu/~ermon/cs228/index.html)创建的本教程的[IPython notebook](https://github.com/kuleshov/cs228-material/blob/master/tutorials/python/cs228-python-tutorial.ipynb)版本。

内容列表：
 * Python
    * 基本数据类型
    * 容器
       * 列表
       * 字典
       * 集合
       * 元组
    * 函数
    * 类    
 * Numpy
   * 数组
   * 访问数组
   * 数据类型
   * 数组的数学
   * 广播
 * SciPy
   * 图像操作
   * MATLAB文件
   * 点之间的距离
 * Matplotlib
   * 绘制图形
   * 子图
   * 图像
   
## Python
   
Python是一种高级的，动态类型的多范型编程语言。很多时候，大家会说Python看起来简直和伪代码一样，这是因为你能够通过很少行数的代码表达出很有力的思想。举个例子，下面是用Python实现的经典的quicksort算法例子：

```python
def quicksort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quicksort(left) + middle + quicksort(right)

print(quicksort([3,6,8,10,1,2,1]))
# Prints "[1, 1, 2, 3, 6, 8, 10]"
```

### Python版本

Python有两个支持的版本，分别是2.7和3.4。这有点让人迷惑，3.0向语言中引入了很多不向后兼容的变化，2.7下的代码有时候在3.4下是行不通的。在这个课程中，我们使用的是2.7版本。

如何查看版本呢？使用python --version命令。
```
>>>python --version
python 2.7.11
```

### 基本数据类型

和大多数编程语言一样，Python拥有一系列的基本数据类型，比如整型、浮点型、布尔型和字符串等。这些类型的使用方式和在其他语言中的使用方式是类似的。

**数字(Numbers)**：整型和浮点型的使用与其他语言类似。

```python
x = 3
print(type(x)) # Prints "<class 'int'>"
print(x)       # Prints "3"
print(x + 1)   # Addition; prints "4"
print(x - 1)   # Subtraction; prints "2"
print(x * 2)   # Multiplication; prints "6"
print(x ** 2)  # Exponentiation; prints "9"
x += 1
print(x)  # Prints "4"
x *= 2
print(x)  # Prints "8"
y = 2.5
print(type(y)) # Prints "<class 'float'>"
print(y, y + 1, y * 2, y ** 2) # Prints "2.5 3.5 5.0 6.25"
```

需要注意的是，Python中没有 x++ 和 x-- 的操作符。

Python也有内置的长整型和复杂数字类型，具体细节可以查看文档。

**布尔型(Booleans)** ： Python实现了所有的布尔逻辑，但用的是英语，而不是我们习惯的操作符（比如&&和||等）。

```python
t = True
f = False
print(type(t)) # Prints "<class 'bool'>"
print(t and f) # Logical AND; prints "False"
print(t or f)  # Logical OR; prints "True"
print(not t)   # Logical NOT; prints "False"
print(t != f)  # Logical XOR; prints "True"
```

**字符串(Strings)** ：Python对字符串的支持非常棒。

```python
hello = 'hello'    # String literals can use single quotes
world = "world"    # or double quotes; it does not matter.
print(hello)       # Prints "hello"
print(len(hello))  # String length; prints "5"
hw = hello + ' ' + world  # String concatenation
print(hw)  # prints "hello world"
hw12 = '%s %s %d' % (hello, world, 12)  # sprintf style string formatting
print(hw12)  # prints "hello world 12"
```
字符串对象有一系列有用的方法，比如：

```python
s = "hello"
print(s.capitalize())  # Capitalize a string; prints "Hello"
print(s.upper())       # Convert a string to uppercase; prints "HELLO"
print(s.rjust(7))      # Right-justify a string, padding with spaces; prints "  hello"
print(s.center(7))     # Center a string, padding with spaces; prints " hello "
print(s.replace('l', '(ell)'))  # Replace all instances of one substring with another;
                                # prints "he(ell)(ell)o"
print('  world '.strip())  # Strip leading and trailing whitespace; prints "world"
```

如果想详细查看字符串方法，请看[文档](https://docs.python.org/3.5/library/stdtypes.html#string-methods)。

### 容器(Containers)

Python包含一些内在容器类型：列表（lists）、字典（dictionaries）、集合（sets）和元组（tuples）。

**列表（Lists）**
列表就是Python中的数组，但是列表长度可变，且能包含不同类型元素。
```python
xs = [3, 1, 2]    # Create a list
print(xs, xs[2])  # Prints "[3, 1, 2] 2"
print(xs[-1])     # Negative indices count from the end of the list; prints "2"
xs[2] = 'foo'     # Lists can contain elements of different types
print(xs)         # Prints "[3, 1, 'foo']"
xs.append('bar')  # Add a new element to the end of the list
print(xs)         # Prints "[3, 1, 'foo', 'bar']"
x = xs.pop()      # Remove and return the last element of the list
print(x, xs)      # Prints "bar [3, 1, 'foo']"
```

列表的细节，同样可以查阅[文档](https://docs.python.org/3.5/tutorial/datastructures.html#more-on-lists)。


**切片(Slicing)** ：为了一次性地获取列表中的元素，Python提供了一种简洁的语法，这就是切片。

```python
nums = list(range(5))     # range is a built-in function that creates a list of integers
print(nums)               # Prints "[0, 1, 2, 3, 4]"
print(nums[2:4])          # Get a slice from index 2 to 4 (exclusive); prints "[2, 3]"
print(nums[2:])           # Get a slice from index 2 to the end; prints "[2, 3, 4]"
print(nums[:2])           # Get a slice from the start to index 2 (exclusive); prints "[0, 1]"
print(nums[:])            # Get a slice of the whole list; prints "[0, 1, 2, 3, 4]"
print(nums[:-1])          # Slice indices can be negative; prints "[0, 1, 2, 3]"
nums[2:4] = [8, 9]        # Assign a new sublist to a slice
print(nums)               # Prints "[0, 1, 8, 9, 4]"
```

我们将在Numpy数组部分再次看到切片。


**循环(Loops)** ：我们可以这样遍历列表中的每一个元素：

```python
animals = ['cat', 'dog', 'monkey']
for animal in animals:
    print(animal)
# Prints "cat", "dog", "monkey", each on its own line.
```

如果想要在循环体内访问每个元素的索引，可以使用内置的enumerate函数
```python
animals = ['cat', 'dog', 'monkey']
for idx, animal in enumerate(animals):
    print('#%d: %s' % (idx + 1, animal))
# Prints "#1: cat", "#2: dog", "#3: monkey", each on its own line
```

**列表综合式(List comprehensions)** ：在编程的时候，我们常常想要将一种数据类型转换为另一种。下面是一个简单例子，将列表中的每个元素变成它的平方。

```python
nums = [0, 1, 2, 3, 4]
squares = []
for x in nums:
    squares.append(x ** 2)
print(squares)   # Prints [0, 1, 4, 9, 16]
```

使用**列表综合式(List comprehensions)**,可以让代码更简单

```python
nums = [0, 1, 2, 3, 4]
squares = [x ** 2 for x in nums]
print(squares)   # Prints [0, 1, 4, 9, 16]
```
**列表综合式(List comprehensions)**,还可以包含条件：
```python
nums = [0, 1, 2, 3, 4]
even_squares = [x ** 2 for x in nums if x % 2 == 0]
print(even_squares)  # Prints "[0, 4, 16]"
```

**字典(Dictionaries)**

字典用来储存（键, 值）对，类似于Java中的Map或Javascript中的对象。你可以这样使用：
```python
d = {'cat': 'cute', 'dog': 'furry'}  # Create a new dictionary with some data
print(d['cat'])       # Get an entry from a dictionary; prints "cute"
print('cat' in d)     # Check if a dictionary has a given key; prints "True"
d['fish'] = 'wet'     # Set an entry in a dictionary
print(d['fish'])      # Prints "wet"
# print(d['monkey'])  # KeyError: 'monkey' not a key of d
print(d.get('monkey', 'N/A'))  # Get an element with a default; prints "N/A"
print(d.get('fish', 'N/A'))    # Get an element with a default; prints "wet"
del d['fish']         # Remove an element from a dictionary
print(d.get('fish', 'N/A')) # "fish" is no longer a key; prints "N/A"
```
你可以道字典的所有信息，请查阅[文档](https://docs.python.org/3.5/library/stdtypes.html#dict)。

**循环Loops** ：在字典中，很容易迭代字典中的键。

```python
d = {'person': 2, 'cat': 4, 'spider': 8}
for animal in d:
    legs = d[animal]
    print('A %s has %d legs' % (animal, legs))
# Prints "A person has 2 legs", "A cat has 4 legs", "A spider has 8 legs"
```

如果你想要访问键和它们对应的值，可以使用items方法：
```python
d = {'person': 2, 'cat': 4, 'spider': 8}
for animal, legs in d.items():
    print('A %s has %d legs' % (animal, legs))
# Prints "A person has 2 legs", "A cat has 4 legs", "A spider has 8 legs"
```

**字典综合式(Dictionary comprehensions)** ：和**列表综合式(List comprehensions)** 类似，但是允许你轻松地构建字典。
```python
nums = [0, 1, 2, 3, 4]
even_num_to_square = {x: x ** 2 for x in nums if x % 2 == 0}
print(even_num_to_square)  # Prints "{0: 0, 2: 4, 4: 16}"
```

**集合Sets**

集合是不同元素的无序集合。作为一个例子，看下面的：
```python
animals = {'cat', 'dog'}
print('cat' in animals)   # Check if an element is in a set; prints "True"
print('fish' in animals)  # prints "False"
animals.add('fish')       # Add an element to a set
print('fish' in animals)  # Prints "True"
print(len(animals))       # Number of elements in a set; prints "3"
animals.add('cat')        # Adding an element that is already in the set does nothing
print(len(animals))       # Prints "3"
animals.remove('cat')     # Remove an element from a set
print(len(animals))       # Prints "2"
```

和前面一样，要知道关于集合的每个细节，查看[文档](https://docs.python.org/3.5/library/stdtypes.html#set)。


**循环Loops** ：在集合中循环的语法和在列表中一样，但是集合是无序的，所以你在访问集合的元素的时候，不能做关于顺序的假设。
```python
animals = {'cat', 'dog', 'fish'}
for idx, animal in enumerate(animals):
    print('#%d: %s' % (idx + 1, animal))
# Prints "#1: fish", "#2: dog", "#3: cat"
```
**集合综合式(Dictionary comprehensions)** ：和**字典综合式(Dictionary comprehensions)** 一样，可以轻松地构建集合：

```python
from math import sqrt
nums = {int(sqrt(x)) for x in range(30)}
print(nums)  # Prints "{0, 1, 2, 3, 4, 5}"
```

**元组Tuples**

元组是一个值的有序列表（不可改变）。从很多方面来说，元组和列表都很相似。和列表最重要的不同在于，元组可以在字典中用作键，还可以作为集合的元素，而列表不行。下面是一个小例子：
```python
d = {(x, x + 1): x for x in range(10)}  # Create a dictionary with tuple keys
t = (5, 6)        # Create a tuple
print(type(t))    # Prints "<class 'tuple'>"
print(d[t])       # Prints "5"
print(d[(1, 2)])  # Prints "1"
```
[文档](https://docs.python.org/3.5/tutorial/datastructures.html#tuples-and-sequences)有更多元组的信息。

### 函数Functions

Python函数使用def来定义函数.例如：
```python
def sign(x):
    if x > 0:
        return 'positive'
    elif x < 0:
        return 'negative'
    else:
        return 'zero'

for x in [-1, 0, 1]:
    print(sign(x))
# Prints "negative", "zero", "positive"
```
我们常常使用可选参数来定义函数：

```python
def hello(name, loud=False):
    if loud:
        print('HELLO, %s!' % name.upper())
    else:
        print('Hello, %s' % name)

hello('Bob') # Prints "Hello, Bob"
hello('Fred', loud=True)  # Prints "HELLO, FRED!"
```

Python函数的更多信息，可以查看[文档](https://docs.python.org/3.5/tutorial/controlflow.html#defining-functions)。

### 类Classes

在Python对于类的语法是直接的：
```python
class Greeter(object):

    # Constructor
    def __init__(self, name):
        self.name = name  # Create an instance variable

    # Instance method
    def greet(self, loud=False):
        if loud:
            print('HELLO, %s!' % self.name.upper())
        else:
            print('Hello, %s' % self.name)

g = Greeter('Fred')  # Construct an instance of the Greeter class
g.greet()            # Call an instance method; prints "Hello, Fred"
g.greet(loud=True)   # Call an instance method; prints "HELLO, FRED!"
```

你可以在[文档](https://docs.python.org/3.5/tutorial/classes.html)中阅读更多Python类的信息

## Numpy

Numpy是Python中用于科学计算的核心库。它提供了高性能的多维数组对象，以及相关工具。

### 数组Arrays

一个numpy数组是一个由不同数值组成的网格。网格中的数据都是同一种数据类型，可以通过非负整型数的元组来访问。维度的数量被称为数组的阶，数组的大小是一个由整型数构成的元组，可以描述数组不同维度上的大小。

我们可以从列表创建数组，然后利用方括号访问其中的元素：
```python
import numpy as np

a = np.array([1, 2, 3])   # Create a rank 1 array
print(type(a))            # Prints "<class 'numpy.ndarray'>"
print(a.shape)            # Prints "(3,)"
print(a[0], a[1], a[2])   # Prints "1 2 3"
a[0] = 5                  # Change an element of the array
print(a)                  # Prints "[5, 2, 3]"

b = np.array([[1,2,3],[4,5,6]])    # Create a rank 2 array
print(b.shape)                     # Prints "(2, 3)"
print(b[0, 0], b[0, 1], b[1, 0])   # Prints "1 2 4"
```

Numpy还提供了很多函数用于创建数组：
```python
import numpy as np

a = np.zeros((2,2))   # Create an array of all zeros
print(a)              # Prints "[[ 0.  0.]
                      #          [ 0.  0.]]"

b = np.ones((1,2))    # Create an array of all ones
print(b)              # Prints "[[ 1.  1.]]"

c = np.full((2,2), 7)  # Create a constant array
print(c)               # Prints "[[ 7.  7.]
                       #          [ 7.  7.]]"

d = np.eye(2)         # Create a 2x2 identity matrix
print(d)              # Prints "[[ 1.  0.]
                      #          [ 0.  1.]]"

e = np.random.random((2,2))  # Create an array filled with random values
print(e)                     # Might print "[[ 0.91940167  0.08143941]
                             #               [ 0.68744134  0.87236687]]"
```
你可以在[文档](http://docs.scipy.org/doc/numpy/user/basics.creation.html#arrays-creation)中阅读数组创建的其他方法。

### 数组索引

Numpy提供了几种方式索引数组

**切片Slicing**：和Python列表类似，numpy数组可以使用切片语法。因为数组可以是多维的，所以你必须为每个维度指定好切片。

```python
mport numpy as np

# Create the following rank 2 array with shape (3, 4)
# [[ 1  2  3  4]
#  [ 5  6  7  8]
#  [ 9 10 11 12]]
a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])

# Use slicing to pull out the subarray consisting of the first 2 rows
# and columns 1 and 2; b is the following array of shape (2, 2):
# [[2 3]
#  [6 7]]
b = a[:2, 1:3]

# A slice of an array is a view into the same data, so modifying it
# will modify the original array.
print(a[0, 1])   # Prints "2"
b[0, 0] = 77     # b[0, 0] is the same piece of data as a[0, 1]
print(a[0, 1])   # Prints "77"
```

你可以混合使用整型索引和切片索引。但这样做会产生一个比原数组低秩的数组。需要注意的是，这里和MATLAB中的情况是不同的：
```python
import numpy as np

# Create the following rank 2 array with shape (3, 4)
# [[ 1  2  3  4]
#  [ 5  6  7  8]
#  [ 9 10 11 12]]
a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])

# Two ways of accessing the data in the middle row of the array.
# Mixing integer indexing with slices yields an array of lower rank,
# while using only slices yields an array of the same rank as the
# original array:
row_r1 = a[1, :]    # Rank 1 view of the second row of a
row_r2 = a[1:2, :]  # Rank 2 view of the second row of a
print(row_r1, row_r1.shape)  # Prints "[5 6 7 8] (4,)"
print(row_r2, row_r2.shape)  # Prints "[[5 6 7 8]] (1, 4)"

# We can make the same distinction when accessing columns of an array:
col_r1 = a[:, 1]
col_r2 = a[:, 1:2]
print(col_r1, col_r1.shape)  # Prints "[ 2  6 10] (3,)"
print(col_r2, col_r2.shape)  # Prints "[[ 2]
                             #          [ 6]
                             #          [10]] (3, 1)"
```

**整型数组的索引(Integer array indexing)** ： 
当我们使用切片索引numpy数组时，得到的数组视图(array view)总是原数组的一个子数组。不同的是，整型数组索引允许我们利用其它数组的数据构建一个新的数组：
```python
import numpy as np

a = np.array([[1,2], [3, 4], [5, 6]])

# An example of integer array indexing.
# The returned array will have shape (3,) and 
print a[[0, 1, 2], [0, 1, 0]]  # Prints "[1 4 5]"

# The above example of integer array indexing is equivalent to this:
print np.array([a[0, 0], a[1, 1], a[2, 0]])  # Prints "[1 4 5]"

# When using integer array indexing, you can reuse the same
# element from the source array:
print a[[0, 0], [1, 1]]  # Prints "[2 2]"

# Equivalent to the previous integer array indexing example
print np.array([a[0, 1], a[0, 1]])  # Prints "[2 2]"
```
整型数组索引的一个有用技巧，可以用来选择或者更改矩阵中每行的一个元素：
```python
import numpy as np

# Create a new array from which we will select elements
a = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])

print(a)  # prints "array([[ 1,  2,  3],
          #                [ 4,  5,  6],
          #                [ 7,  8,  9],
          #                [10, 11, 12]])"

# Create an array of indices
b = np.array([0, 2, 0, 1])

# Select one element from each row of a using the indices in b
print(a[np.arange(4), b])  # Prints "[ 1  6  7 11]"

# Mutate one element from each row of a using the indices in b
a[np.arange(4), b] += 10

print(a)  # prints "array([[11,  2,  3],
          #                [ 4,  5, 16],
          #                [17,  8,  9],
          #                [10, 21, 12]])
```
**布尔型数组的索引(Boolean array indexing)** ：Boolean array indexing 可以让你选择数组中任意元素。这种索引方式经常用于选取数组中满足某些条件的元素，例如：
```python
import numpy as np

a = np.array([[1,2], [3, 4], [5, 6]])

bool_idx = (a > 2)   # Find the elements of a that are bigger than 2;
                     # this returns a numpy array of Booleans of the same
                     # shape as a, where each slot of bool_idx tells
                     # whether that element of a is > 2.

print(bool_idx)      # Prints "[[False False]
                     #          [ True  True]
                     #          [ True  True]]"

# We use boolean array indexing to construct a rank 1 array
# consisting of the elements of a corresponding to the True values
# of bool_idx
print(a[bool_idx])  # Prints "[3 4 5 6]"

# We can do all of the above in a single concise statement:
print(a[a > 2])     # Prints "[3 4 5 6]"
```

为了简短，我们省略了numpy array indexing的许多细节如果你想知道更多，可以看[文档](http://docs.scipy.org/doc/numpy/reference/arrays.indexing.html)。

### 数据类型

每个Numpy数组都是数据类型相同的元素组成的网格。Numpy提供了很多的数值数据类型(numeric datatypes)用于创建数组。当你创建数组的时候，Numpy会尝试猜测数组的数据类型，但创建数组的函数通常使用可选参数显式地指定数据类型，例如：
```python
import numpy as np

x = np.array([1, 2])   # Let numpy choose the datatype
print(x.dtype)         # Prints "int64"

x = np.array([1.0, 2.0])   # Let numpy choose the datatype
print(x.dtype)             # Prints "float64"

x = np.array([1, 2], dtype=np.int64)   # Force a particular datatype
print(x.dtype)                         # Prints "int64"
```

关于numpy datatypes的全部信息可查看[文档](http://docs.scipy.org/doc/numpy/reference/arrays.dtypes.html)。

### 数组数学(Array math)

基本的数学函数操作是针对数组中元素逐个进行的，（在numpy模块中）既可以是操作符重载，也可以是函数：
```python
import numpy as np

x = np.array([[1,2],[3,4]], dtype=np.float64)
y = np.array([[5,6],[7,8]], dtype=np.float64)

# Elementwise sum; both produce the array
# [[ 6.0  8.0]
#  [10.0 12.0]]
print(x + y)
print(np.add(x, y))

# Elementwise difference; both produce the array
# [[-4.0 -4.0]
#  [-4.0 -4.0]]
print(x - y)
print(np.subtract(x, y))

# Elementwise product; both produce the array
# [[ 5.0 12.0]
#  [21.0 32.0]]
print(x * y)
print(np.multiply(x, y))

# Elementwise division; both produce the array
# [[ 0.2         0.33333333]
#  [ 0.42857143  0.5       ]]
print(x / y)
print(np.divide(x, y))

# Elementwise square root; produces the array
# [[ 1.          1.41421356]
#  [ 1.73205081  2.        ]]
print(np.sqrt(x))
```
和MATLAB不同，```*```是元素逐个相乘，而不是矩阵乘法。我们用dot来进行向量的内积、矩阵乘以向量和矩阵乘法。dot既可以是numpy module（模块）的函数也可以是数组对象的实例方法
```python
import numpy as np

x = np.array([[1,2],[3,4]])
y = np.array([[5,6],[7,8]])

v = np.array([9,10])
w = np.array([11, 12])

# Inner product of vectors; both produce 219
print(v.dot(w))
print(np.dot(v, w))

# Matrix / vector product; both produce the rank 1 array [29 67]
print(x.dot(v))
print(np.dot(x, v))

# Matrix / matrix product; both produce the rank 2 array
# [[19 22]
#  [43 50]]
print(x.dot(y))
print(np.dot(x, y))
```
Numpy提供了很多有用的函数对数组进行计算，其中最常用的一个是sum：
```python
import numpy as np

x = np.array([[1,2],[3,4]])

print(np.sum(x))  # Compute sum of all elements; prints "10"
print(np.sum(x, axis=0))  # Compute sum of each column; prints "[4 6]"
print(np.sum(x, axis=1))  # Compute sum of each row; prints "[3 7]"
```
你可以在[文档](http://docs.scipy.org/doc/numpy/reference/routines.math.html)里找到numpy提供的数学函数的完整列表。

除了使用数组进行数学计算的函数，我们经常需要reshape数组（改变数组形状）或操纵数组的元素，其中最简单的例子就是对矩阵的转置操作，为了转置矩阵，只要使用数组对象的T属性。
```python
import numpy as np

x = np.array([[1,2], [3,4]])
print(x)    # Prints "[[1 2]
            #          [3 4]]"
print(x.T)  # Prints "[[1 3]
            #          [2 4]]"

# Note that taking the transpose of a rank 1 array does nothing:
v = np.array([1,2,3])
print(v)    # Prints "[1 2 3]"
print(v.T)  # Prints "[1 2 3]"
```

Numpy还提供了更多操作数组的函数，完整列表可以查看[文档](http://docs.scipy.org/doc/numpy/reference/routines.array-manipulation.html)。

### 广播Broadcasting

广播是一个强有力的机制，使得numpy可以对不同形状的数组进行算术运算。我们常常会有一个小的矩阵和一个大的矩阵，我们经常用小的矩阵对大的矩阵做一些操作。

例如，我们想要把一个常向量加到矩阵的每一行，我们可以这样做：
```python
import numpy as np

# We will add the vector v to each row of the matrix x,
# storing the result in the matrix y
x = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
v = np.array([1, 0, 1])
y = np.empty_like(x)   # Create an empty matrix with the same shape as x

# Add the vector v to each row of the matrix x with an explicit loop
for i in range(4):
    y[i, :] = x[i, :] + v

# Now y is the following
# [[ 2  2  4]
#  [ 5  5  7]
#  [ 8  8 10]
#  [11 11 13]]
print(y)
```
这是可行的，但当矩阵x非常大，用显式循环来计算就会变得很慢很慢。注意到，将向量v加到矩阵x的每一行，等价于将多个向量v垂直堆积(stacking multiple copies)起来构成一个矩阵vv，然后对x和vv进行逐个元素的求和，我们这样来实现
 ```python
 import numpy as np

# We will add the vector v to each row of the matrix x,
# storing the result in the matrix y
x = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
v = np.array([1, 0, 1])
vv = np.tile(v, (4, 1))   # Stack 4 copies of v on top of each other
print(vv)                 # Prints "[[1 0 1]
                          #          [1 0 1]
                          #          [1 0 1]
                          #          [1 0 1]]"
y = x + vv  # Add x and vv elementwise
print(y)  # Prints "[[ 2  2  4
          #          [ 5  5  7]
          #          [ 8  8 10]
          #          [11 11 13]]"
 ```
Numpy broadcasting(广播)允许我们进行这种计算而不需要实际创建多个v的拷贝，考虑使用这种广播的版本：
```python
import numpy as np

# We will add the vector v to each row of the matrix x,
# storing the result in the matrix y
x = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
v = np.array([1, 0, 1])
y = x + v  # Add v to each row of x using broadcasting
print(y)  # Prints "[[ 2  2  4]
          #          [ 5  5  7]
          #          [ 8  8 10]
          #          [11 11 13]]"
```
这一行y = x + v 能正常工作，即使x 具有形状 shape (4, 3) 而v 具有形状 (3,)。借助于广播，这一行能够工作就好像v 实际具有形状(4, 3)、和运算时逐个元素进行的。

对两个数组使用广播机制要遵守下列规则：
```
如果数组的秩不同，使用1来将秩较小的数组进行扩展，直到两个数组的尺寸的长度都一样。
如果两个数组在某个维度上的长度是一样的，或者其中一个数组在该维度上长度为1，那么我们就说这两个数组在该维度上是相容的。
如果两个数组在所有维度上都是相容的，他们就能使用广播。
如果两个输入数组的尺寸不同，那么注意其中较大的那个尺寸。因为广播之后，两个数组的尺寸将和那个较大的尺寸一样。
在任何一个维度上，如果一个数组的长度为1，另一个数组长度大于1，那么在该维度上，就好像是对第一个数组进行了复制。
如果上述解释看不明白，可以读一读文档和这个解释。译者注：强烈推荐阅读文档中的例子。
```

如何这个解释还不够清楚，可以查看[文档](http://docs.scipy.org/doc/numpy/user/basics.broadcasting.html)和[这个解释](http://wiki.scipy.org/EricsBroadcastingDoc)

支持广播机制的函数是全局函数。可以在[文档](http://docs.scipy.org/doc/numpy/reference/ufuncs.html#available-ufuncs)中查找全局函数的列表。

下面是广播的一些例子：
```python
import numpy as np

# Compute outer product of vectors
v = np.array([1,2,3])  # v has shape (3,)
w = np.array([4,5])    # w has shape (2,)
# To compute an outer product, we first reshape v to be a column
# vector of shape (3, 1); we can then broadcast it against w to yield
# an output of shape (3, 2), which is the outer product of v and w:
# [[ 4  5]
#  [ 8 10]
#  [12 15]]
print(np.reshape(v, (3, 1)) * w)

# Add a vector to each row of a matrix
x = np.array([[1,2,3], [4,5,6]])
# x has shape (2, 3) and v has shape (3,) so they broadcast to (2, 3),
# giving the following matrix:
# [[2 4 6]
#  [5 7 9]]
print(x + v)

# Add a vector to each column of a matrix
# x has shape (2, 3) and w has shape (2,).
# If we transpose x then it has shape (3, 2) and can be broadcast
# against w to yield a result of shape (3, 2); transposing this result
# yields the final result of shape (2, 3) which is the matrix x with
# the vector w added to each column. Gives the following matrix:
# [[ 5  6  7]
#  [ 9 10 11]]
print((x.T + w).T)
# Another solution is to reshape w to be a column vector of shape (2, 1);
# we can then broadcast it directly against x to produce the same
# output.
print(x + np.reshape(w, (2, 1)))

# Multiply a matrix by a constant:
# x has shape (2, 3). Numpy treats scalars as arrays of shape ();
# these can be broadcast together to shape (2, 3), producing the
# following array:
# [[ 2  4  6]
#  [ 8 10 12]]
print(x * 2)
```
广播机制能够让你的代码更简洁更迅速，你应该尽可能使用它！

### Numpy 文档（Documentation）

简单的概述涉触及了你需要了解的numpy中的一些重要内容，但远不止如此。可以查阅numpy参考手册[numpy reference](http://docs.scipy.org/doc/numpy/reference/)查询更多信息。

## SciPy
Numpy提供了高性能的多维数组，以及计算和操作数组的基本工具。在此基础上SciPy提供了大量的操作numpy数组的函数，在不同类型的科学和工程计算中非常有用。

熟悉SciPy的最好方法就是浏览[文档](http://docs.scipy.org/doc/scipy/reference/index.html)。我们会突出对本课程有用的部分。

### 图像操作Image operations
SciPy提供了一些操作图像的基本函数。比如，它提供了将图像从硬盘读入到数组的函数，也提供了将数组中数据写入硬盘成为图像的函数。这是一个说明这些函数的简单例子：
```python
from scipy.misc import imread, imsave, imresize

# Read an JPEG image into a numpy array
img = imread('assets/cat.jpg')
print(img.dtype, img.shape)  # Prints "uint8 (400, 248, 3)"

# We can tint the image by scaling each of the color channels
# by a different scalar constant. The image has shape (400, 248, 3);
# we multiply it by the array [1, 0.95, 0.9] of shape (3,);
# numpy broadcasting means that this leaves the red channel unchanged,
# and multiplies the green and blue channels by 0.95 and 0.9
# respectively.
img_tinted = img * [1, 0.95, 0.9]

# Resize the tinted image to be 300 by 300 pixels.
img_tinted = imresize(img_tinted, (300, 300))

# Write the tinted image back to disk
imsave('assets/cat_tinted.jpg', img_tinted)
```

|                                             |                                                       |
| -----------------------------------------   | ---------------------------------------------------   | 
|  ![](https://cs231n.github.io/assets/cat.jpg) | ![](https://cs231n.github.io/assets/cat_tinted.jpg) | 
    


左边是原始图片，右边是变色和改变大小的图片。


### MATLAB文件

函数scipy.io.loadmat和scipy.io.savemat允许你读和写MATLAB文件。可在[文档](http://docs.scipy.org/doc/scipy/reference/io.html)查看它们。

**点之间的距离**

SciPy定义了一些有用的函数，可以计算集合中点之间的距离。

函数scipy.spatial.distance.pdist能够计算集合中所有两点之间的距离：
```python
import numpy as np
from scipy.spatial.distance import pdist, squareform

# Create the following array where each row is a point in 2D space:
# [[0 1]
#  [1 0]
#  [2 0]]
x = np.array([[0, 1], [1, 0], [2, 0]])
print(x)

# Compute the Euclidean distance between all rows of x.
# d[i, j] is the Euclidean distance between x[i, :] and x[j, :],
# and d is the following array:
# [[ 0.          1.41421356  2.23606798]
#  [ 1.41421356  0.          1.        ]
#  [ 2.23606798  1.          0.        ]]
d = squareform(pdist(x, 'euclidean'))
print(d)
```

关于此函数的细节请阅读[文档](http://docs.scipy.org/doc/scipy/reference/generated/scipy.spatial.distance.pdist.html)。

函数scipy.spatial.distance.cdist可以计算不同集合中点的距离，具体请查看[文档](http://docs.scipy.org/doc/scipy/reference/generated/scipy.spatial.distance.cdist.html)。

## Matplotlib

Matplotlib是一个作图库。这里简要介绍matplotlib.pyplot模块，提供了和MATLAB的类似的绘图功能。

### 绘图Plotting

matplotlib库中最重要的函数是Plot，该函数允许你做出2D图形，这是一个简单例子：
```python
import numpy as np
import matplotlib.pyplot as plt

# Compute the x and y coordinates for points on a sine curve
x = np.arange(0, 3 * np.pi, 0.1)
y = np.sin(x)

# Plot the points using matplotlib
plt.plot(x, y)
plt.show()  # You must call plt.show() to make graphics appear.
```
运行上面代码会产生下面的plot(图)：

![](https://cs231n.github.io/assets/sine.png)

只需要少量额外的工作，就可以轻松地一次画多个线，加上标题、标签（legend）、坐标轴标签等。
```python
import numpy as np
import matplotlib.pyplot as plt

# Compute the x and y coordinates for points on sine and cosine curves
x = np.arange(0, 3 * np.pi, 0.1)
y_sin = np.sin(x)
y_cos = np.cos(x)

# Plot the points using matplotlib
plt.plot(x, y_sin)
plt.plot(x, y_cos)
plt.xlabel('x axis label')
plt.ylabel('y axis label')
plt.title('Sine and Cosine')
plt.legend(['Sine', 'Cosine'])
plt.show()
```
![](https://cs231n.github.io/assets/sine_cosine.png)

可以在[文档](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.plot)中阅读更多关于plot的内容。

### 子图(Subplots)

可以使用subplot函数来在一幅图中画不同的东西,这是一个例子
```python
import numpy as np
import matplotlib.pyplot as plt

# Compute the x and y coordinates for points on sine and cosine curves
x = np.arange(0, 3 * np.pi, 0.1)
y_sin = np.sin(x)
y_cos = np.cos(x)

# Set up a subplot grid that has height 2 and width 1,
# and set the first such subplot as active.
plt.subplot(2, 1, 1)

# Make the first plot
plt.plot(x, y_sin)
plt.title('Sine')

# Set the second subplot as active, and make the second plot.
plt.subplot(2, 1, 2)
plt.plot(x, y_cos)
plt.title('Cosine')

# Show the figure.
plt.show()
```
~[](https://cs231n.github.io/assets/sine_cosine_subplot.png)

关于subplot的更多细节，可以阅读[文档](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.subplot)。

### 图像

你可以使用imshow函数来显示图像，这是一个例子：
```python
import numpy as np
from scipy.misc import imread, imresize
import matplotlib.pyplot as plt

img = imread('assets/cat.jpg')
img_tinted = img * [1, 0.95, 0.9]

# Show the original image
plt.subplot(1, 2, 1)
plt.imshow(img)

# Show the tinted image
plt.subplot(1, 2, 2)

# A slight gotcha with imshow is that it might give strange results
# if presented with data that is not uint8. To work around this, we
# explicitly cast the image to uint8 before displaying it.
plt.imshow(np.uint8(img_tinted))
plt.show()
```

![](https://cs231n.github.io/assets/cat_tinted_imshow.png)

#### 觉得文章对您有用，帮忙点击博客上面的广告哦！谢谢！


