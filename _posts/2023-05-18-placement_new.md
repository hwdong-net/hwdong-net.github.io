---
layout:       post
title:        "Placement new operator in C++"
subtitle:     "build a object in place with 'Placement new operator' "
date:         2023-05-18 01:05:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: false
tags:
    - AI
---

In C++, when you create an object using the new keyword, the memory for the object is allocated on the heap, and the constructor is called to initialize the object. However, in some cases, you may want to allocate memory beforehand and then construct an object directly in that pre-allocated memory. This is where placement new comes into play.

Placement new is a technique that allows you to construct an object at a specific memory address. It is accomplished by using a specific form of the new operator along with the address where you want the object to be constructed. The syntax for placement new is as follows:
```cpp
new (address) Type(args);
```
Here, address is the memory address where you want the object to be constructed, Type is the type of the object, and args are the arguments to be passed to the object's constructor.

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

In this example, we allocate memory for a MyClass object using a char buffer of the appropriate size. Then, we use the placement new syntax to construct a MyClass object directly in that memory address. The constructor is called, and we can access the object using the pointer. When we're done with the object, we can explicitly call the destructor using the ~MyClass() syntax.

It's worth noting that if you use placement new to construct an object, you must ensure that you explicitly call the destructor (~MyClass()) before deallocating the memory or reusing it for other purposes.

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
In this example, the allocateMemory function represents a custom memory allocator that provides memory from a specific source, such as a memory pool. You can then use the placement new operator to construct a MyClass object directly in the allocated memory.

2. Array of Objects:

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

In this example, we allocate memory using ::operator new[] to hold an array of MyClass objects. We then use the placement new operator within a loop to construct each object in the allocated memory. After construction, we can access and use the objects as needed. Finally, we explicitly call the destructor for each object before releasing the memory using ::operator delete[].

These examples highlight some situations where the placement new operator can be useful. Remember that placement new should be used judiciously and with caution, as it involves low-level memory management and requires explicit destructor calls when appropriate.

