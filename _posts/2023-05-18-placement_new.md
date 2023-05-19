---
layout:       post
title:        "'Placement new operator' and emplace_back() in C++"
subtitle:     "emplace_back() of vector is used to construct an element in-place at the end of the vector. "
date:         2023-05-18 01:05:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: false
tags:
    - AI
--- 

### 'Placement new operator' in C++


In C++, when you create an object using the '**new**' keyword, the memory for the object is allocated on the heap, and the constructor is called to initialize the object. However, in some cases, you may want to allocate memory beforehand and then construct an object directly in that pre-allocated memory. This is where placement new comes into play.

Placement new is a technique that allows you to construct an object at a specific memory address. It is accomplished by using a specific form of the '**new**' operator along with the address where you want the object to be constructed. The syntax for 'placement **new**' is as follows:
```cpp
new (address) Type(args);
```
Here, **address** is the memory address where you want the object to be constructed, **Type** is the type of the object, and **args** are the arguments to be passed to the object's constructor.

Here's an example to illustrate its usage:
```cpp
#include <iostream>

class MyClass {
public:
  MyClass(int value) : data(value) {
    std::cout << "Constructor called. Value: " << data << std::endl;
  }

  ~MyClass() {
    std::cout << "Destructor called. Value: " << data << std::endl;
  }

private:
  int data;
};

int main() {
  char buffer[sizeof(MyClass)];  // Pre-allocated memory for MyClass

  MyClass* object = new (buffer) MyClass(42);  // Placement new

  // Access the object using the pointer
  std::cout << "Object value: " << object->getData() << std::endl;

  // Destroy the object explicitly (not necessary in this case)
  object->~MyClass();

  return 0;
}
```

In this example, we allocate memory for a **MyClass** object using a '**char**' buffer of the appropriate size. Then, we use the placement new syntax to construct a **MyClass** object directly in that memory address. The constructor is called, and we can access the object using the pointer. When we're done with the object, we can explicitly call the destructor using the **~MyClass()** syntax.

It's worth noting that if you use placement new to construct an object, you must ensure that you explicitly call the destructor (**~MyClass()**) before deallocating the memory or reusing it for other purposes.

Please note that the usage of placement new requires caution and is generally more advanced. It should only be used when necessary, such as in low-level memory management scenarios or when dealing with specific requirements, such as custom memory allocators.


Here are a few examples that demonstrate the application of the placement new operator in different scenarios:

**1. Custom Memory Allocation:**

Placement new can be useful when you want to allocate memory from a custom memory pool or a specific memory region. This can be beneficial in embedded systems or real-time applications where you have strict memory constraints.
```cpp
#include <iostream>

// Custom memory allocator
void* allocateMemory(size_t size) {
    // Implementation specific to your custom memory allocator
    // ...
}

class MyClass {
public:
    MyClass(int value) : data(value) {
        std::cout << "Constructor called. Value: " << data << std::endl;
    }

    ~MyClass() {
        std::cout << "Destructor called. Value: " << data << std::endl;
    }

private:
    int data;
};

int main() {
    void* memory = allocateMemory(sizeof(MyClass));

    MyClass* object = new (memory) MyClass(42);

    // ...

    object->~MyClass();

    return 0;
}
```
In this example, the '**allocateMemory**' function represents a custom memory allocator that provides memory from a specific source, such as a memory pool. You can then use the placement new operator to construct a '**MyClass**' object directly in the allocated memory.

**2. Array of Objects:**

Placement new can also be used to construct an array of objects in a pre-allocated memory region.
```cpp
#include <iostream>

class MyClass {
public:
    MyClass(int value) : data(value) {
        std::cout << "Constructor called. Value: " << data << std::endl;
    }

    ~MyClass() {
        std::cout << "Destructor called. Value: " << data << std::endl;
    }

    int getData() const {
        return data;
    }

private:
    int data;
};

int main() {
    const size_t numObjects = 5;
    void* memory = ::operator new[](numObjects * sizeof(MyClass));

    MyClass* objects = static_cast<MyClass*>(memory);

    for (size_t i = 0; i < numObjects; ++i) {
        new (&objects[i]) MyClass(i + 1);
    }

    // Access the objects
    for (size_t i = 0; i < numObjects; ++i) {
        std::cout << "Object " << i << " value: " << objects[i].getData() << std::endl;
    }

    // Destroy the objects explicitly
    for (size_t i = 0; i < numObjects; ++i) {
        objects[i].~MyClass();
    }

    ::operator delete[](memory);

    return 0;
}
```

In this example, we allocate memory using '**::operator new[]**' to hold an array of '**MyClass**' objects. We then use the placement new operator within a loop to construct each object in the allocated memory. After construction, we can access and use the objects as needed. Finally, we explicitly call the destructor for each object before releasing the memory using '**::operator delete[]**'.

These examples highlight some situations where the placement new operator can be useful. Remember that placement new should be used judiciously and with caution, as it involves low-level memory management and requires explicit destructor calls when appropriate.

### emplace_back() vs push_back() in C++ Vectors

In C++, emplace_back() and push_back() are both member functions of the std::vector class that are used to add elements to the end of a vector. The main difference between them lies in how they construct or add elements to the vector.

**1. push_back():** Push_back adds a new element at the end. It first creates a temporary object by calling a constructor.

Here is an example implementation of the emplace_back() function in C++:
```cpp
void push_back(const T& value) {
   if (size_ == capacity_) {
       reserve(2 * capacity_); // double the capacity of the vector
   }
   data_[size_] = value; // insert the new element at the end of the vector
   size_++; // increment the size of the vector
}
```
   This function push_back() is used to add an element to the end of the vector by making a copy or move of the provided object. It accepts an argument of the type that the vector holds and appends a copy of that object to the vector. If the object being added is an lvalue, it will be copied, and if it is an rvalue, it will be moved.
```cpp
std::vector<int> numbers;
int x = 10;
numbers.push_back(x); // Copy of x is added to the vector
numbers.push_back(20); // Move the rvalue 20 into the vector
```

**2. emplace_back():** It also Inserts a new element at the end. It does not create a temporary object. The object is directly created in the vector. Due to this, the efficiency is increased.

Here is an example implementation of the emplace_back() function in C++:
```cpp
template <typename... Args>
void emplace_back(Args&&... args) {
   if (size_ == capacity_) {
       reserve(2 * capacity_); // double the capacity of the vector
   }
   new (&data_[size_]) T(std::forward<Args>(args)...); // construct the new element in place
   size_++; // increment the size of the vector
}
```
 
This function is used to construct an element in-place at the end of the vector. It forwards the provided arguments to the constructor of the element type, which means it constructs the object directly inside the vector without making any additional copies. This can be more efficient than push_back() because it eliminates the need for unnecessary copies or moves.

```cpp
std::vector<std::string> words;
words.emplace_back("hello"); // Construct the string "hello" in-place
words.emplace_back(5, 'a'); // Construct a string with 5 'a' characters in-place
```
In summary, push_back() adds a copy or move of an existing object to the vector, while emplace_back() constructs an object directly inside the vector using provided arguments. emplace_back() is generally more efficient when constructing objects with multiple parameters, as it avoids unnecessary copying or moving.

