---
layout:       post
title:        "Overview of python| python tutorial 1"
subtitle:     "Overview of python | python tutorial 1"
date:         2020-05-08 11:00:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - python
---


## Overview of python
+ What and why
+ python interpreter 
+ Interactive Mode vs Script Mode
`
       print()
`
+ Pycharm vs Jupyter notebook

### What and why

+ Python is a high-level, interpreted, interactive and object-oriented scripting language
+ Python was developed by Guido van Rossum in the late eighties and early nineties at the National Research Institute for Mathematics and + Computer Science in the Netherlands.
+ Python can be used in automatic、web developing、data science、 deep learning and AI, etc

**Automatic**: automate tedious tasks such as updating spreadsheets or renaming files on your computer. improve your work efficiency
https://realpython.com/what-can-i-do-with-python/#1-automate-the-boring-stuff

**web developing**: Web frameworks that are based on Python like Django and Flask have recently become very popular for web development.
These web frameworks help you create server-side code (backend code) in Python. That’s the code that runs on your server, as opposed to on users’ devices and browsers (front-end code). 

![](./imgs/Django.png)

![](./imgs/Flask.png)

https://www.freecodecamp.org/news/what-can-you-do-with-python-the-3-main-applications-518db9a68a78/

**Data analysis** is a process of inspecting, cleansing, transforming and modeling data with the goal of discovering useful information, informing conclusions and supporting decision-making. 

![](./imgs/da.png)


**Data visualization** is the graphical representation of information and data. By using visual elements like charts, graphs, and maps, data visualization tools provide an accessible way to see and understand trends, outliers, and patterns in data.
https://www.tableau.com/learn/articles/data-visualization

![](./imgs/Visual.png)
One of the most popular libraries for data visualization is Matplotlib.
Some other libraries such as seaborn is based on it


**Machine learning** is a method of data analysis that automates analytical model building. It is a branch of artificial intelligence based on the idea that systems can learn from data, identify patterns and make decisions with minimal human intervention.
https://www.sas.com/en_us/insights/analytics/machine-learning.html

![](./imgs/Face_reg.png)

**Deep learning** is an artificial intelligence function that imitates the workings of the human brain in processing data and creating patterns for use in decision making. Deep learning is a subset of machine learning in artificial intelligence (AI) that has networks capable of learning unsupervised from data that is unstructured or unlabeled. Also known as deep neural learning or deep neural network.

![](./imgs/DeepFace.png)

https://www.investopedia.com/terms/d/deep-learning.asp

Python has many advantages,such as:

+ Easy to  learn and understand
+ A broad standard library
+ Widely used in different area and by different people, not only programmer
+ Top language

![](./imgs/Tiob.png)

![](./imgs/IEEE.png)

### python interpreter 
An interpreter executes the statements of code “one-by-one”,  whereas the compiler executes the code entirely and lists all possible errors at a time.
The interpreter turn each line of  code into an intermediate code usually called the byte code. The executes the an intermediate code. So you can see the execution result of each command immediately.
To run python, you just need a python interpreter (install and set up the path )

### Interactive Mode vs Script Mode

Interactive Mode: Invoking the interpreter without passing a script file as a parameter 
     $ python

Script Mode: Invoking the interpreter with a script parameter begins execution of the script and continues until the script is finished. 
     $ python test.py

### print()
A function used to print out information.
Function is a named code-block
```
     print(“hello”)
     print(3+5)
     print(“hi,”, “I am hwdong”)
```

### script file (module file)

Contain all python commands
File ended with   .py

For example,
```python
# test.py

print(“hello”)
print(3+5)

```

Where Lines started with # is comments not commands

### Pycharm vs Jupyter notebook

Pycharm:  intelligent code completion, on-the-fly error checking and quick-fixes
Jupyter: web-based interactive computing platform. The notebook combines live code, equations, narrative text, visualizations, interactive dashboards and other media. 

 Install Jupyter:
 ```
pip install jupyter
```
Anaconda：packages management platform for data science

