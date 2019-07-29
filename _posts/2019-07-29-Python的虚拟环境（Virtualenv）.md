---
layout:       post
title:        Intellij IDEA安装创建JavaFX程序
subtitle:     install intellij IDEA and create JavaFX Application
date:         2019-07-29 07:53:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - IT    
---   

Python分为python 2和python 3，它们各自又有多个不同版本（如python3.4，python3.5.6），如果系统直接使用不同的python版本，会引起冲突。另外即使同一个版本的python，其中的
各个包package或库也有会有不同的版本，而python中一个包或库只能由一个版本，如果想根据需要使用同一个包的不同版本，怎么办？

上述这些情况，都需要使用python虚拟环境（Virtualenv），即创建一些独立的、互不干扰的、干净的虚拟环境，虚拟环境之间互不干扰。因此，
一般开发python程序都建议创建虚拟环境，在虚拟环境下进行python编程。

virtualenv就是用来创建和管理虚拟环境的。

首先需要安装virtualenv，右击命令行程序"command prompt"以管理员身份运行。（否则会出现错误：“Consider using the `--user` option or check the permissions.”）
```
pip3 install -U pip virtualenv
```
如果是python2，则用
```
pip install -U pip virtualenv
```
如果不想以管理员身份运行pip，可以加上:--user。如：
```
pip3 install -U pip virtualenv --user
```
然后按下面步骤创建一个虚拟环境。

1）先为虚拟环境创建一个工作目录如“my_venv_dir”。如在命令行输入：
```
mkdir my_venv_dir
cd my_venv_dir
```
2) 在目录“my_venv_dir”下创建一个虚拟环境，如“my_venv”。
```
virtualenv --no-site-packages  my_venv
```
参数--no-site-packages表示已经安装到系统Python环境中的所有第三方包都不会复制过来，即创建的虚拟环境中将不包含任何任何第三方包。默认情况下，会将系统中安装的第三方包也安装在虚拟环境中。

也可以用-p选项指定某个版本的python。如：

```
virtualenv --no-site-packages -p python3 my_venv
```

3）激活虚拟环境
```
.\venv\Scripts\activate
```
如果是conda环境，则用
```
source activate my_venv
```
命令提示符变了，有个(venv)前缀，表示当前环境是一个名为venv的Python环境。
```
(my_venv) D:\my_venv_dir>
```

4）可以用pip在虚拟环境下安装自己需要的第三方包。如：
```
pip install --upgrade pip
pip install numpy
pip install scipy
pip install -U matplotlib
pip install jupyter 
pip install tensorflow-gpu==2.0.0-beta1
```
或
```
python -m pip install numpy
python -m pip install scipy
python -m pip install -U matplotlib
```
也可以先用 "pip list" 命令列出当前已经安装了哪些包？
```
pip list
```
将显示：
```
Package    Version
---------- -------
pip        19.2.1
setuptools 40.8.0
virtualenv 16.7.2
```

虚拟环境环境下，用pip安装的包都被安装到这个虚拟环境中，系统Python环境或其他虚拟环境都不受任何影响。

5)退出虚拟环境很简单，只要执行下面的命令
```
deactivate
```
如果是conda环境，则用
```
source deactivate
```

